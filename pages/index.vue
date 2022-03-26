<template>
    <main class="grid grid-cols-1">
        <section class="py-20 text-center text-20">
            <div class="fadeUp">Empty page</div>
        </section>
        <section class="grid grid-cols-1 md:grid-cols-2 gap-4 md:gap-8 p-8">
            <Photo :src_dsk="require('~/assets/img/image1.webp')" class="mx-auto h-200 w-full" />
            <div class="text-4">Text</div>
        </section>
        <section class="py-20">
            <div class="text-center text-12">API data example:</div>
            <div class="w-300 mx-auto">
                <div v-for="todo in todos" :key="todo.id" :class="{ 'text-green-600 font-bold': todo.completed }">
                    {{ todo.title }}
                </div>
            </div>
        </section>
    </main>
</template>

<script>
import gsap from 'gsap'
import { ScrollTrigger } from 'gsap/dist/ScrollTrigger.min'

gsap.registerPlugin(ScrollTrigger)

export default {
    mounted() {
        this.$fadeUpInit()
    },
    async asyncData({ $axios, params, error }) {
        try {
            let response = await $axios.$get(`https://jsonplaceholder.typicode.com/todos`)
            // console.log(response)

            return {
                todos: response.slice(0, 20),
            }
        } catch (err) {
            console.log(err)
            if ($axios.isCancel(error)) {
                console.log('Aborted')
            } else {
                error({ statusCode: 500 })
                return
            }
        }
    },
}
</script>
