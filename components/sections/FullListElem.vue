<template>
    <div class="bg-black h-60 relative overflow-hidden group" :class="mob_hover ? '_hover' : ''">
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
import gsap from 'gsap'
import { ScrollTrigger } from 'gsap/dist/ScrollTrigger.min'

gsap.registerPlugin(ScrollTrigger)

export default {
    props: ['item'],
    data: () => ({
        mob_hover: false,
    }),
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

        InitGSAP() {
            this.scrollTrigger = ScrollTrigger.create({
                trigger: this.$el,
                start: 'top 50%',
                end: 'bottom 50%',
                // markers: true,
                onEnter: () => {
                    this.mob_hover = true
                },
                onEnterBack: () => {
                    this.mob_hover = true
                },
                onLeave: () => {
                    this.mob_hover = false
                },
                onLeaveBack: () => {
                    this.mob_hover = false
                },
            })
        },
    },

    mounted() {
        let is_mobile = window.matchMedia(`(max-width: ${this.$getRootVar('--phone-w')})`).matches
        if (is_mobile) this.InitGSAP()
    },
}
</script>

<style lang="scss" scoped>
@media (max-width: $phone) {
    ._hover {
        .group-hover\:scale-110 {
            transform: scale(1.1);
        }
        .group-hover\:opacity-90 {
            opacity: 0.9;
        }
    }
}
</style>
