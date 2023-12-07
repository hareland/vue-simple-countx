# vue-simple-countx

A simple countdown/countup that is easily injectable and configurable.

## usage

```vue

<template>
  <vue-simple-countx
      :value="1000"
      :steps="98"
      :timeout="10"
  />
</template>
<script lang="ts" setup>
  import {VueSimpleCountX} from "vue-simple-countx/src";
</script>
```

## options

| **Parameter** | **Description**                                                                                     | **Required**/**Default** |
|---------------|-----------------------------------------------------------------------------------------------------|--------------------------|
| :value        | What to count to (positive/negative)                                                                | Yes                      |
| :steps        | How many steps to jump each iteration (if there is less than steps remaining, it will count slower) | No, default `10`         |
| :timeout      | Similar to setInterval by using milliseconds for iterations.                                        | No, default `100`        |
