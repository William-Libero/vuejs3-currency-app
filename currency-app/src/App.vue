<template>
  <div v-if="listenQuotes.length > 0" class="grid justify-items-center my-9">
    <WatchListQuotes :listenQuotes="listenQuotes" @unlisten="onUnlisten"/>
  </div>
  <div class="grid justify-items-center my-9">
    <span class="text-3xl mb-3">Todas as moedas:</span>
    <ListQuotes :quotes="quotes" :listenQuotes="listenQuotes" @listen="onListen" @unlisten="onUnlisten"/>
  </div>
</template>

<script>
import {onMounted, reactive, toRefs} from 'vue'
import api from '@/services/api'
import ListQuotes from './components/ListQuotes.vue'
import WatchListQuotes from './components/WatchListQuotes.vue'

export default {
  name: 'App',
  components: {
    ListQuotes,
    WatchListQuotes
  },
  setup() {
    const data = reactive({
      quotes: {},
      listenQuotes: []
    });

    onMounted(async () => {
      const response = await api.all();
      data.quotes = response.data;
    });

    function onListen(code) {
      data.listenQuotes.push(code);
    }

    function onUnlisten(code) {
      data.listenQuotes = data.listenQuotes.filter(key => key != code);
    }

    return {...toRefs(data), onListen, onUnlisten}
  }
}
</script>
