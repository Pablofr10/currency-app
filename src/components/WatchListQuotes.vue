<template>
  <ListQuotes :quotes="quotes" :listen-quotes="listenQuotes" />
  <cite class="text-small">
    <div class="mt-2 text-right">
      Atualizará novamente em <b>{{ nextUpdateTime }} segundos</b>
    </div>
  </cite>
</template>

<script>
import { ref } from "vue";
import ListQuotes from "./ListQuotes.vue";
export default {
  components: { ListQuotes },
  props: {
    listenQuotes: { type: Array, required: true },
  },

  setup() {
    const quotes = ref({});
    const nextUpdateTime = ref(30);

    return { quotes, nextUpdateTime };
  },
};
</script>
