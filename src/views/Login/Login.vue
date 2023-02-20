<template>
  <div>
    hello login
  </div>
</template>

<script setup lang="ts">
import { useStore } from '@/store'

const store = useStore()

store.dispatch('users/login').then((res) => {
  console.log(res.data);
})
</script>

<style scoped lang="scss"></style>