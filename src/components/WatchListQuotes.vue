<template>
  <ListQuotes :quotes="quotes" />
  <cite class="text-small">
    <div class="mt-2 text-right">Atualizará novamente <b>x segundos</b></div>
  </cite>
</template>

<script>
import { ref } from "vue";
import ListQuotes from "./ListQuotes.vue";
export default {
  components: { ListQuotes },

  setup() {
    const quotes = ref({});

    return { quotes };
  },
};
</script>
