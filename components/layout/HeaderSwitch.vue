<template>
    <div id="header_switch"></div>
</template>

<script>
import gsap from 'gsap'
import { ScrollTrigger } from 'gsap/dist/ScrollTrigger.min'

gsap.registerPlugin(ScrollTrigger)

export default {
    data: () => ({
        scrollTrigger: null,
        header: null,
    }),
    methods: {
        InitGSAP() {
            this.scrollTrigger = ScrollTrigger.create({
                trigger: '#header_switch',
                start: '-100 top',
                end: '-100 top',
                // markers: true,
                onLeave: () => {
                    this.header.is_showcase = false
                },
                onLeaveBack: () => {
                    this.header.is_showcase = true
                },
            })
        },
    },
    mounted() {
        this.header = document.querySelector('#header')
        this.header = this.header ? this.header.__vue__ : null

        this.InitGSAP()
    },
    beforeDestroy() {
        if (this.scrollTrigger && this.scrollTrigger.kill) this.scrollTrigger.kill()
    },
}
</script>

<style lang="scss" scoped></style>
