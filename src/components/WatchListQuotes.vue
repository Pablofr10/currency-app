<template>
  <ListQuotes
    :quotes="quotes"
    :listen-quotes="listenQuotes"
    @unlisten="onUnlisten"
  />
  <cite class="text-small">
    <div class="mt-2 text-right">
      Atualizará novamente em <b>{{ nextUpdateTime }} segundos</b>
    </div>
  </cite>
</template>

<script>
import { onMounted, onUnmounted, reactive, ref, toRef, watch } from "vue";
import ListQuotes from "./ListQuotes.vue";
import api from "@/services/api";

const CURRENT_UPDATE_TIME = 30;

export default {
  components: { ListQuotes },
  props: {
    listenQuotes: { type: Array, required: true },
  },
  emits: ["unlisten"],
  setup(props, emit) {
    const interval = ref(null);
    const quotes = ref({});
    const nextUpdateTime = ref(CURRENT_UPDATE_TIME);

    const methods = reactive({
      onUnlisten(code) {
        emit("unlisten", code);
      },

      restartInterval() {
        clearInterval(interval.value);
        nextUpdateTime.value = CURRENT_UPDATE_TIME;
        interval.value = setInterval(() => {
          nextUpdateTime.value -= 1;
          if (nextUpdateTime.value === 0) {
            nextUpdateTime.value = CURRENT_UPDATE_TIME;
            this.refresh();
          }
        }, 1000);
      },

      async refresh() {
        const { data } = await api.listen(props.listenQuotes);
        quotes.value = data;
      },
    });

    onMounted(() => {
      methods.refresh();
      methods.restartInterval();
    });

    onUnmounted(() => {
      clearInterval(interval.value);
    });

    watch(props, () => {
      methods.refresh();
      methods.restartInterval();
    });

    return {
      ...toRef(methods),
      quotes,
      nextUpdateTime,
    };
  },
};
</script>
