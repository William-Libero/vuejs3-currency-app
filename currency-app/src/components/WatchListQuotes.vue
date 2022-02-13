<template>
    <ListQuotes :quotes="quotes" :listen-quotes="listenQuotes" @unlisten="onUnlisten"/>
    <cite>Atualizará novamente em <b>{{nextUpdateTime}} segundos</b></cite>
</template>

<script>
import {onUnmounted, onMounted, reactive, ref, toRefs, watch} from 'vue'
import ListQuotes from './ListQuotes'
import api from '@/services/api'

const CURRENT_UPDATE_TIME = 10;

export default {
    components: { ListQuotes },
    props: {
        listenQuotes: {type: Array, required: true}
    },
    emits: ["unlisten"],
    setup(props, {emit}) {
        const quotes = ref({});
        const nextUpdateTime = ref(CURRENT_UPDATE_TIME);
        const interval = ref(null);

        const methods = reactive({
            onUnlisten(code) {
                emit('unlisten', code)
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
            async refresh(){
                const {data} = await api.listen(props.listenQuotes);
                quotes.value = data
            }
        });

        onMounted(() => {
            methods.refresh();
            methods.restartInterval();
        });

        onUnmounted(() => {
            clearInterval(interval.value);
        });

        watch(props,() => {
            methods.refresh();
            methods.restartInterval();
        });

        return {...toRefs(methods), quotes, nextUpdateTime}
    },
}
</script>
