<template>
    <table>
        <thead class="bg-gray-200 dark:bg-gray-700">
            <tr class="text-left">
                <th scope="col" class="py-3 px-6 text-xs font-medium tracking-wider text-left text-gray-700 uppercase dark:text-gray-400">Código</th>
                <th scope="col" class="py-3 px-6 text-xs font-medium tracking-wider text-left text-gray-700 uppercase dark:text-gray-400">Nome</th>
                <th scope="col" class="py-3 px-6 text-xs font-medium tracking-wider text-left text-gray-700 uppercase dark:text-gray-400">Máximo</th>
                <th scope="col" class="py-3 px-6 text-xs font-medium tracking-wider text-left text-gray-700 uppercase dark:text-gray-400">Minimo</th>
                <th scope="col" class="py-3 px-6 text-xs font-medium tracking-wider text-left text-gray-700 uppercase dark:text-gray-400">Variação</th>
                <th scope="col" class="py-3 px-6 text-xs font-medium tracking-wider text-left text-gray-700 uppercase dark:text-gray-400"></th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="(quote, key) in quotes" :key="key" class="bg-white border-b dark:bg-gray-800 dark:border-gray-700">
                <td class="pr-6">{{key}}</td>
                <td class="pr-3">{{quote.name}}</td>
                <td class="pr-3">{{quote.high}}</td>
                <td class="pr-3">{{quote.low}}</td>
                <td class="text-center rounded-lg" :class="{'bg-red-600 text-white': quote.pctChange < 0, 'bg-green-600 text-white': quote.pctChange > 0}">{{quote.pctChange}} %</td>
                <td class="px-3">
                    <a v-if="!listenQuotes.includes(key)" @click="$emit('listen', key)" href="#" data-tooltip="Seguir">+</a>
                    <a v-else @click="$emit('unlisten', key)" href="#" data-tooltip="Remover">-</a>
                </td>
            </tr>
        </tbody>
    </table>
</template>

<script>
export default {
    props: {
        quotes: {type: Object, required: true},
        listenQuotes: {type: Array, required: true},
    },
    emits: ['listen', 'unlisten'],
}
</script>