<script lang="ts" setup>
import { useSlots, ref, onMounted, inject, computed } from 'vue'

const slots = useSlots()
const thisElement = ref<HTMLElement | null>(null)

const emit = defineEmits<{
  optionSelected: [newValue: any, label: string]
}>()

const props = defineProps<{
  value?: any
  disabled?: boolean
  selected?: boolean
}>()

const label = ref<string>()

const parentSelectOption: ((value: any, label: string) => void) | undefined = inject('selectOption')

const selectThisOption = () => {
  if (!parentSelectOption) throw new Error('Select option method not provided by the parent.')
  parentSelectOption(props.value, <string>label.value)
}

const handleClick = () => {
  if (!props.disabled) {
    emit('optionSelected', props.value, label.value ? label.value : '')
    selectThisOption()
  }
}

onMounted(() => {
  label.value = slots.default ? (slots.default()[0]?.children as string) : ''
  if (props.selected) {
    selectThisOption()
  }
})

const classObject = computed(() => ({
  disabled: props.disabled,
}))
</script>

<template>
  <li @click="handleClick" ref="thisElement" :class="classObject">
    <slot />
  </li>
</template>

<style scoped>
li {
  cursor: pointer;
  white-space: nowrap;
}

.disabled {
  cursor: default;
  color: #7a7a7a;
}
</style>
