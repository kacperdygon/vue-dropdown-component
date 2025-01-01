<template>

  <p>Press the button to open dropdown menu</p>
  <p>Selected value: {{ selectedDropdownValue }}</p>

  <DropdownMenu defaultValue="1" defaultLabel="Select Option" v-model='selectedDropdownValue'>
    <DropdownItem value="1" disabled>Option 1</DropdownItem>
    <DropdownItem value="2" selected>Option 2</DropdownItem>
    <DropdownItem value="3">Option 3</DropdownItem>
    <DropdownItem value="4">Option 4</DropdownItem>
    <DropdownItem value="5">Option 5</DropdownItem>
  </DropdownMenu>
</template>

<script setup lang="ts">
import { DropdownMenu, DropdownItem } from '@/vue-dropdown/index.ts'
import { ref } from 'vue'

const selectedDropdownValue = ref<number>(0);

</script>

<style>
#app {
  display:flex;
  flex-direction: column;
  width:100%;
  height:90vh;
  justify-content: center;
  align-items: center;
}
</style>
