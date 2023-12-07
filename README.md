# vue-simple-countx

A simple countdown/countup that is easily injectable and configurable.


## Usage

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