<script lang="ts" setup>
import { onMounted, onBeforeUnmount, ref, nextTick } from 'vue'

const props = defineProps<{
  defaultValue: any,
  defaultLabel: any,
}>();

const emit = defineEmits<{
  optionChanged: [newValue: any];
  menuOpened: [];
}>();

const model = defineModel();

const ulElement = ref<HTMLElement | null>(null);
const currentValue = ref<any>(props.defaultValue);
const currentLabel = ref<string>(props.defaultLabel);
const isOpen = ref<boolean>(false);

function onClick() {
  isOpen.value = !isOpen.value;
}

const selectOption = (value: any, label: string) => {
  currentLabel.value = label;
  currentValue.value = value;
  isOpen.value = false;
  model.value = value;
  emit("optionChanged", value);
};

const handleSelectEvent = (event: Event) => {
  const customEvent = event as CustomEvent;
  selectOption(customEvent.detail.optionValue, customEvent.detail.optionLabel);
  customEvent.stopPropagation();
}

onMounted(() => {
  nextTick(() => {
    console.log('ulElement:', ulElement.value);
    if (ulElement.value) {
      ulElement.value.addEventListener('selectEvent', handleSelectEvent);
    } else {
      throw new Error('ulElement not assigned!');
    }
  });
});

</script>

<template>
  <div>
    <button @click="onClick">
      {{ currentLabel }}
    </button>
    <ul ref="ulElement" v-show='isOpen'>
      <slot></slot>
    </ul>
  </div>
</template>
