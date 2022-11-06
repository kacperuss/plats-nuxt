<template>
    <div></div>
</template>

<script>
export default {
    data: () => ({
        game: null,
    }),

    methods: {
        async fetch_stuff() {
            try {
                let response = await Storyblok.get('cdn/stories')
                console.log(response)
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
