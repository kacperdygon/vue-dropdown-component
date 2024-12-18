<script lang="ts" setup>
import { useSlots, ref, onMounted } from 'vue'

const slots = useSlots();
const thisElement = ref<HTMLElement | null>(null);

const emit = defineEmits<{
  optionSelected: [newValue: any, label: string ];
}>();

const props = defineProps<{
  value: any;
}>();
const label = ref<string | null>();

const handleClick = () => {
  emitDomSelectEvent();
  emit('optionSelected', props.value, label.value ? label.value : '');
};

function emitDomSelectEvent() {

  const customEvent = new CustomEvent('selectEvent', {
    detail: {
      optionValue: props.value,
      optionLabel: label.value,
    },
    bubbles: true,
    composed: true,
  });

  if (thisElement.value) {
    thisElement.value.dispatchEvent(customEvent); // Dispatch the event
  } else {
    throw new Error('This element ref not set!');
  }
}

onMounted(() => {
  label.value = slots.default ? slots.default()[0]?.children as string : '';
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
