<template>
  <div class="apply-title">
    <el-button type="primary" @click="handleOpen">添加审批</el-button>
    <el-space>
      <el-input v-model="searchWord" placeholder="请输入搜索关键词" />
      <el-button type="primary" icon="search">搜索</el-button>
      <el-divider direction="vertical"></el-divider>
      <el-radio-group v-model="approverType">
        <el-radio-button label="全部" />
        <el-radio-button label="待审批" />
        <el-radio-button label="已通过" />
        <el-radio-button label="未通过" />
      </el-radio-group>
    </el-space>
  </div>
  <div class="apply-table">
    <el-table :data="pageApplyList" border style="width: 100%">
      <el-table-column prop="applicantname" label="申请人" width="180" />
      <el-table-column prop="reason" label="审批事由" width="180" />
      <el-table-column prop="time" label="时间">
        <template #default="scope">
          {{ scope.row.time.join(' - ') }}
        </template>
      </el-table-column>
      <el-table-column prop="note" label="备注" />
      <el-table-column prop="approvername" label="审批人" width="180" />
      <el-table-column prop="state" label="状态" width="180" />
    </el-table>
    <el-pagination small background layout="prev, pager, next" :total="applyList.length" :page-size="pageSize"
      @current-change="handleChange" />
  </div>
  <el-dialog v-model="dialogVisible" title="添加审批" width="500px" :before-close="handleClose">
    <el-form ref="ruleFormRef" :model="ruleForm" :rules="rules" label-width="80px">
      <el-form-item label="审批人" prop="approvername">
        <el-select v-model="ruleForm.approvername" placeholder="请选择审批人">
          <el-option v-for="item in approver" :key="(item._id as string)" :value="(item.name as string)"
            :label="(item.name as string)" />
        </el-select>
      </el-form-item>
      <el-form-item label="审批事由" prop="reason">
        <el-select v-model="ruleForm.reason" placeholder="请选择审批事由">
          <el-option value="年假" label="年假" />
          <el-option value="事假" label="事假" />
          <el-option value="病假" label="病假" />
          <el-option value="外出" label="外出" />
          <el-option value="补签卡" label="补签卡" />
        </el-select>
      </el-form-item>
      <el-form-item label="时间" prop="time">
        <el-date-picker v-model="ruleForm.time" type="datetimerange" start-placeholder="起始日期" end-placeholder="结束日期" />
      </el-form-item>
      <el-form-item label="备注" prop="note">
        <el-input v-model="ruleForm.note" :autosize="{ minRows: 4, maxRows: 6 }" type="textarea" placeholder="请输入备注" />
      </el-form-item>
    </el-form>
    <template #footer>
      <el-button @click="resetForm(ruleFormRef)">重置</el-button>
      <el-button type="primary" @click="submitForm(ruleFormRef)">提交</el-button>
    </template>
  </el-dialog>
</template>

<script setup lang="ts">
import { ref, computed, reactive } from 'vue'
import { useStore } from '@/store';
import type { DateModelType, FormInstance, FormRules } from 'element-plus'

interface ApplyList {
  applicantid: string,
  applicantname: string,
  approverid: string,
  approvername: string,
  note: string,
  reason: string,
  time: [DateModelType, DateModelType]
}

const store = useStore()
const defaultType = '全部'
const approverType = ref(defaultType)
const searchWord = ref('')
const usersInfos = computed(() => store.state.users.infos)
const approver = computed(() => usersInfos.value.approver as { [index: string]: unknown }[])
const applyList = computed(() => store.state.checks.applyList.filter((v) => (v.state === approverType.value || defaultType === approverType.value) && (v.note as string).includes(searchWord.value)))
const pageSize = ref(2)
const pageCurrent = ref(1)
const dialogVisible = ref(false)

const pageApplyList = computed(() => applyList.value.slice((pageCurrent.value - 1) * pageSize.value, pageCurrent.value * pageSize.value))

const handleChange = (value: number) => {
  pageCurrent.value = value
}

const handleClose = () => {
  dialogVisible.value = false
}

const ruleFormRef = ref<FormInstance>()

const ruleForm = reactive<ApplyList>({
  applicantid: '',
  applicantname: '',
  approverid: '',
  approvername: '',
  note: '',
  reason: '',
  time: ['', '']
})

const rules = reactive<FormRules>({
  email: [
    { required: true, message: '请输入邮箱', trigger: 'blur' },
    { type: 'email', message: '请输入正确的邮箱地址', trigger: 'blur' }
  ],
  pass: [
    { required: true, message: '请输入密码', trigger: 'blur' },
  ]
})

const handleOpen = () => {
  dialogVisible.value = true
}

const submitForm = (formEl: FormInstance | undefined) => {
  if (!formEl) return
  formEl.validate((valid) => {
    if (valid) {
      console.log(ruleForm);
    } else {
      return false
    }
  })
}

const resetForm = (formEl: FormInstance | undefined) => {
  if (!formEl) return
  formEl.resetFields()
}
</script>

<style scoped lang="scss">
.apply-title {
  margin: 20px;
  display: flex;
  justify-content: space-between;
}

.apply-table {
  margin: 10px;

  .el-pagination {
    float: right;
    margin: 10px;
  }
}
</style>