<template>
  <div class="container grid-lg">
    <input v-model="model" />
    <h1>
      {{ model }}
    </h1>
  </div>
</template>

<script>
import { ref } from "vue";
export default {
  name: "App",
  setup() {
    const model = ref("Hello Wold");
    return { model };
  },
};
</script>
