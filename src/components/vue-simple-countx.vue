<template>
  {{ currentValue }}
</template>
<script lang="ts" setup>
import {computed, onBeforeMount, onBeforeUnmount, ref} from "vue";

const props = withDefaults(defineProps<{
  timeout?: number;
  value: number;
  steps?: number;
}>(), {
  timeout: 100,
  steps: 1,
});

let iterations = 0;
let interval: NodeJS.Timeout;

const initialValue = ref(props.value);
const currentValue = ref(0);
const targetValue = computed(() => Number(props.value));

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

  return nextValue + (props.steps + 1);
};

const mount = () => {
  interval = setInterval(intervalHandler, props.timeout)
}

const unmount = () => {
  if (interval) {
    clearInterval(interval);
  }
}

const intervalHandler = () => {
  if (interval && currentValue.value === targetValue.value) {
    unmount();
    return;
  }

  iterations++;

  currentValue.value = getNextValue();
}

const reset = () => {
  unmount();
  iterations = 0;
  currentValue.value = initialValue.value;
  mount();
}

onBeforeMount(mount);
onBeforeUnmount(unmount);

defineExpose({
  reset,
  currentValue,
})
</script>