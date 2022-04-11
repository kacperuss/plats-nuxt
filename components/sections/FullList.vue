<template>
    <section class="py-16">
        <div class="grid grid-cols-4 gap-8 px-16">
            <div v-for="(item, i) in game_list" :key="i" class="bg-black h-60 relative overflow-hidden group">
                <div class="w-full h-full group-hover:scale-110 transition-transform duration-300">
                    <Photo :src_dsk="item.front_img || item.back_img" class="w-full h-full" />
                </div>
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
                        {{ item.title }}
                    </div>
                    <div class="absolute top-0 left-0 flex gap-4 text-5 py-3 px-4">
                        <div
                            v-for="(console, j) in item.list"
                            :key="`console_${j}`"
                            class=""
                            @mouseenter="onLogoHover(i, j)"
                        >
                            <ConsoleDict type="logo" :code="console.console" />
                        </div>
                    </div>
                    <div class="absolute bottom-0 right-0 text-5 py-3 px-4">
                        {{ timestampToDate(item.list[item.show_date || 0].date) }}
                    </div>
                </div>
            </div>
        </div>
    </section>
</template>

<script>
export default {
    props: ['game_list'],
    methods: {
        timestampToDate(ts) {
            let date = new Date(ts * 1000)
            let year = date.getUTCFullYear()
            let month = date.getUTCMonth() + 1
            let day = date.getUTCDate()

            month = month < 10 ? '0' + month : month
            day = day < 10 ? '0' + day : day

            return year + '-' + month + '-' + day
        },

        onLogoHover(game_index, icon_index) {
            // console.log(game_index, icon_index)
            // console.log(this.game_list[game_index].show_date)

            this.game_list[game_index].show_date = icon_index
            // console.log(this.game_list[game_index].show_date)
        },
    },
}
</script>

<style lang="scss" scoped></style>
