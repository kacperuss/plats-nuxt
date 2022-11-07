<template>
    <main class="min-h-screen pb-24">
        <div class="h-120 bg-black opacity-60">
            <Photo v-if="game" :src_dsk="$getImg(game, false)" class="w-full h-full" />
        </div>
        <HeaderSwitch />
        <div class="px-60" v-if="game && plats">
            <div class="">
                <div class="relative">
                    <div class="absolute top-0 -translate-y-1/2 right-0">
                        <div class="absolute top-0 right-8 w-80 h-1/2 __backdrop"></div>
                        <div class="relative flex items-end p-4">
                            <div class="relative">
                                <div
                                    class="__title absolute w-276 bottom-44 right-0 text-20 font-bold pr-14 text-right"
                                >
                                    {{ game.content.full_title || game.name }}
                                </div>
                            </div>
                            <div class="__img w-80 h-80 bg-black">
                                <Photo v-if="game" :src_dsk="$getImg(game, true)" class="w-full h-full" />
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="pt-24 text-12 font-bold">
                <div class="">Plat'd {{ num_to_text(plats.length) }}:</div>
            </div>
            <div class="pt-12 text-6">
                <div class="flex items-center pb-2" v-for="(plat, i) in plats" :key="i">
                    <div class="">{{ plat.Date.split(' ')[0] }}</div>
                    <div class="px-16 flex text-12 gap-12 flex-1">
                        <ConsoleDict class="w-15" type="logo" :code="plat.Platform" />
                        <ConsoleDict type="name" :code="plat.Platform" />
                    </div>
                    <a target="_blank" :href="plat.Trophy_List_Link" class="block text-8 py-2 px-6">
                        <i class="fas fa-external-link-alt"></i>
                    </a>
                </div>
            </div>
            <div class="pt-24" v-if="related">
                <div class="text-12 font-bold">Related Games:</div>
                <div class="pt-12 grid grid-cols-3 gap-24">
                    <div class="h-60 bg-black relative group" v-for="(rel, i) in related" :key="`rel_${i}`">
                        <Photo v-if="game" :src_dsk="$getImg(rel, true)" class="w-full h-full" />
                        <div
                            class="
                                absolute
                                w-full
                                h-full
                                top-0
                                left-0
                                bg-lightBlack
                                opacity-0
                                group-hover:opacity-90
                                transition-opacity
                                duration-300
                            "
                        >
                            <div class="absolute top-1/2 left-0 -translate-y-1/2 w-full text-center text-8 font-bold">
                                {{ rel.name }}
                            </div>
                        </div>
                        <a class="block absolute top-0 left-0 w-full h-full" :href="`/game/${rel.slug}/`"></a>
                    </div>
                </div>
            </div>
        </div>
    </main>
</template>

<script>
import StoryblokClient from 'storyblok-js-client'

const Storyblok = new StoryblokClient({
    accessToken: 'BQSh00DYhQzNBbRdEsQzHQtt',
    https: true,
    cache: {
        clear: 'auto',
        type: 'memory',
    },
})
export default {
    data: () => ({
        game: null,
        plats: null,

        related: null,

        plat_list: [],
    }),

    methods: {
        num_to_text(num) {
            return { 1: 'once', 2: 'twice', 3: 'thrice' }[num] || `${num} times`
        },

        async fetch_stuff() {
            try {
                let response = await Storyblok.get(`cdn/stories/games/${this.$route.query.game}`)
                this.game = response.data.story

                // === PLAT LIST ===

                this.plat_list = []
                let page = 1
                while (true) {
                    let response = await Storyblok.get('cdn/stories', {
                        starts_with: 'plats/',
                        per_page: 100,
                        page: page,
                    })
                    if (response.data.stories.length == 0) break

                    this.plat_list.push(...response.data.stories)
                    page += 1
                }

                this.plats = this.$getPlats(this.plat_list, this.game.uuid)

                // === RELATED GAMES ===

                if (this.game.content.related_games && this.game.content.related_games.length) {
                    let rel_response = await Storyblok.get(`cdn/stories`, {
                        by_uuids: this.game.content.related_games.join(),
                    })
                    this.related = rel_response.data.stories
                }

                //
            } catch (err) {
                console.log(err)
                this.$nuxt.error({ statusCode: 500 })
                return
            }
        },
    },

    mounted() {
        if (!this.$route.query || !this.$route.query.game) {
            window.location.replace('/')
            return
        }
        this.fetch_stuff()
    },
}
</script>

<style lang="scss" scoped>
.__backdrop {
    backdrop-filter: blur(5px);
    box-shadow: -8px -8px 12px #0008;
}
.__title {
    filter: drop-shadow(-4px -4px 2px #000);
}
.__img {
    box-shadow: -6px -6px 8px #0008;
}
</style>
