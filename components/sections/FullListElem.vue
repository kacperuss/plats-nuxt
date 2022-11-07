<template>
    <div class="bg-black h-60 relative overflow-hidden group">
        <div class="w-full h-full group-hover:scale-110 transition-transform duration-300">
            <Photo :src_dsk="$getImg(item.game, true)" class="w-full h-full" />
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
                {{ item.game.name }}
            </div>
            <div class="absolute top-0 left-0 flex text-5 gap-3 py-3 px-4">
                <ConsoleDict type="logo" :code="item.content.Platform" />
                <ConsoleDict type="name" :code="item.content.Platform" />
            </div>
            <div class="absolute bottom-0 right-0 text-5 py-3 px-4">
                {{ item.content.Date.split(' ')[0] }}
            </div>
        </div>
        <a class="block absolute top-0 left-0 w-full h-full" :href="`/game/?game=${item.game.slug}`"></a>
    </div>
</template>

<script>
export default {
    props: ['item'],
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
    },
}
</script>

<style lang="scss" scoped>
.__show_on_hover {
    position: absolute;
    top: 0;
    left: 100%;
    white-space: nowrap;
    background: black;
    pointer-events: none;
    z-index: 1;
    opacity: 0;
    visibility: hidden;
    transition: opacity 0.2s, visibility 0.2s;
}
.__console_icon:hover {
    .__show_on_hover {
        opacity: 1;
        visibility: visible;
    }
}
</style>
