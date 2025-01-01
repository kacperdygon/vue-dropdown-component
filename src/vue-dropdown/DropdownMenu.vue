<script lang="ts" setup>
import { ref, provide } from 'vue'

const props = defineProps<{
  defaultValue: any
  defaultLabel: any
}>()

const emit = defineEmits<{
  optionChanged: [newValue: any, newLabel: string]
  menuOpened: []
}>()

const model = defineModel()

const ulElement = ref<HTMLElement | null>(null)
const currentValue = ref<any>(props.defaultValue)
const currentLabel = ref<string>(props.defaultLabel)
const isOpen = ref<boolean>(false)

function onClick() {
  isOpen.value = !isOpen.value
}

const selectOption = (value: any, label: string) => {
  currentLabel.value = label
  currentValue.value = value
  isOpen.value = false
  model.value = value
  emit('optionChanged', value, label)
}

provide<(value: any, label: string) => void>('selectOption', selectOption)
</script>

<template>
  <div>
    <button @click="onClick">
      {{ currentLabel }}
    </button>
    <ul ref="ulElement" v-show="isOpen">
      <slot></slot>
    </ul>
  </div>
</template>

<style scoped>

div {
  position:relative;
}

ul {
  list-style: none;
  margin: 0;
  padding: 0;
  position:absolute;

}
</style>
