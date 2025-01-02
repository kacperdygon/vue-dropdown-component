<template>
  <p>Press the button to open dropdown menu</p>
  <p>Selected value: {{ selectedDropdownValue }}</p>

  <DropdownMenu
    default-value="1"
    default-label="Select Option"
    v-model="selectedDropdownValue"
    align-width
  >
    <DropdownItem value="1" disabled>Option 1</DropdownItem>
    <DropdownItem value="2" selected>Option 2</DropdownItem>
    <DropdownItem value="3">Long option name</DropdownItem>
    <DropdownItem value="4">Option 4</DropdownItem>
    <DropdownItem value="5">Option 5</DropdownItem>
  </DropdownMenu>
</template>

<script setup lang="ts">
import { DropdownMenu, DropdownItem } from '@/vue-dropdown/index.ts'
import { ref } from 'vue'

const selectedDropdownValue = ref<number>(0)
</script>

<style scoped></style>
