<script lang="ts" setup>
import { useSlots, ref, onMounted, inject } from 'vue'

const slots = useSlots()
const thisElement = ref<HTMLElement | null>(null)

const emit = defineEmits<{
  optionSelected: [newValue: any, label: string]
}>()

const props = defineProps<{
  value?: any
}>()

const label = ref<string>()

const selectOption = inject<(value: any, label: string) => void>('selectOption')

const handleClick = () => {
  emit('optionSelected', props.value, label.value ? label.value : '')
  if (!selectOption) throw new Error('Select option method not passed properly.')
  selectOption(props.value, <string>label.value)
}

onMounted(() => {
  label.value = slots.default ? (slots.default()[0]?.children as string) : ''
})
</script>

<template>
  <li @click="handleClick" ref="thisElement">
    <slot />
  </li>
</template>

<style scoped>
li {
  cursor: pointer;
}
</style>
