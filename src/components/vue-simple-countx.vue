<template>
  {{ currentValue }}
</template>
<script lang="ts" setup>
import {computed, onBeforeUnmount, onMounted, onUnmounted, ref, useSlots} from "vue";

const props = withDefaults(defineProps<{
  timeout?: number;
  value: number;
  steps?: number;
}>(), {
  timeout: 100,
  steps: 1,
});

const iterations = ref(0);
let interval: NodeJS.Timeout;

const targetValue = computed(() => Number(props.value))
const currentValue = ref(0);
const getNextValue = () => {
  if (targetValue.value > 0) {
    const nextValue = currentValue.value + props.steps;
    if (nextValue < targetValue.value) {
      return nextValue;
    }

    return nextValue - (props.steps - 1);
  }

  const nextValue = currentValue.value - props.steps;
  if (nextValue >= targetValue.value) {
    return nextValue;
  }

  return nextValue + (props.steps + 1)
};

const intervalHandler = () => {
  if (interval && currentValue.value === targetValue.value) {
    clearInterval(interval)
    return;
  }

  iterations.value++;

  currentValue.value = getNextValue();
}

const mount = () => {
  interval = setInterval(intervalHandler, props.timeout)
}

const reset = () => {
  if (interval) {
    clearInterval(interval)
  }

  iterations.value = 0;
  mount();
}

const unmount = () => {
  if (interval) {
    clearInterval(interval);
  }
}


onMounted(mount);
onBeforeUnmount(unmount);

defineExpose({
  reset,
  getNextValue,
})
</script>