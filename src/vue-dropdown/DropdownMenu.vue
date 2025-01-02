<script lang="ts" setup>
import { ref, provide, computed } from 'vue'

const props = defineProps<{
  defaultValue: any
  defaultLabel: any
  alignWidth?: boolean
}>()

const emit = defineEmits<{
  optionChanged: [newValue: any, newLabel: string]
  menuOpened: []
}>()

const model = defineModel()

const currentValue = ref<any>(props.defaultValue)
const currentLabel = ref<string>(props.defaultLabel)
const isOpen = ref<boolean>(false)
const ulRef = ref<HTMLElement>()
const buttonRef = ref<HTMLElement>()

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

const buttonWidth = computed(() => {
  if (!props.alignWidth) {
    return 'auto'
  }
  if (!ulRef.value) {
    throw new Error('Dropdown menu ref not set.')
  }
  ulRef.value.style.display = 'block'
  const returnValue = ulRef.value?.getBoundingClientRect().width + 'px'
  ulRef.value.style.display = 'none'
  return returnValue
})
</script>

<template>
  <div>
    <button @click="onClick" class="dropdown-button" ref="buttonRef">
      {{ currentLabel }}
    </button>
    <ul ref="ulRef" v-show="isOpen" class="dropdown-menu">
      <slot></slot>
    </ul>
  </div>
</template>

<style scoped>
div {
  position: relative;
}

.dropdown-menu {
  position: absolute;
}

.dropdown-button {
  all: unset;
  background-color: #d3d3ff;
  cursor: pointer;
  width: v-bind(buttonWidth);
}

ul {
  list-style: none;
  margin: 0;
  padding: 0;
  background-color: #ff9999;
}
</style>
