<template>
    <main class="grid grid-cols-1">
        <div id="showcase_anchor"></div>
        <Favs v-if="fav_list" :fav_list="fav_list" />
        <div v-else class="bg-black h-screen"></div>
        <HeaderSwitch />
        <div id="fulllist_anchor"></div>
        <Filters v-if="plat_list" />
        <FullList v-if="plat_list" :game_list="plat_list" />
    </main>
</template>

<script>
import StoryblokClient from 'storyblok-js-client'

const Storyblok = new StoryblokClient({
    accessToken: 'BQSh00DYhQzNBbRdEsQzHQtt',
    cache: {
        clear: 'auto',
        type: 'memory',
    },
})
export default {
    data: () => ({
        fav_list: null,

        game_list: null,
        plat_list: null,
    }),

    methods: {
        async fetch_stuff() {
            try {
                // === GAME LIST ===

                this.game_list = []
                let page = 1
                while (true) {
                    let response = await Storyblok.get('cdn/stories', {
                        starts_with: 'games/',
                        per_page: 100,
                        page: page,
                    })
                    if (response.data.stories.length == 0) break

                    this.game_list.push(...response.data.stories)
                    page += 1
                }

                // === PLAT LIST ===

                this.plat_list = []
                page = 1
                while (true) {
                    let response = await Storyblok.get('cdn/stories', {
                        starts_with: 'plats/',
                        per_page: 100,
                        page: page,
                    })
                    if (response.data.stories.length == 0) break

                    this.plat_list.push(
                        ...response.data.stories.map((x) => {
                            x['game'] = this.$getGame(this.game_list, x.content.Game)
                            return x
                        })
                    )
                    page += 1
                }

                // === FAV LIST ===

                let resp_favs = await Storyblok.get('cdn/stories/global')
                this.fav_list = resp_favs.data.story.content.Favourites.map((x) => {
                    let y = this.$getGame(this.game_list, x)
                    y['plats'] = this.$getPlats(this.plat_list, x)
                    return y
                })
                // console.log(this.fav_list)

                //
            } catch (err) {
                console.log(err)
                this.$nuxt.error({ statusCode: 500 })
                return
            }
        },
    },

    mounted() {
        this.fetch_stuff()
    },
}
</script>

<style lang="scss" scoped></style>
