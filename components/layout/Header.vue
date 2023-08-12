<template>
    <section id="header">
        <div class="__header" :class="is_showcase ? 'showcase' : ''">
            <a href="/" class="flex">
                <div class="w-[2em]"><i class="fas fa-trophy"></i></div>
                <div class="">KP</div>
            </a>
            <div class="flex justify-between items-center gap-6 md:gap-12" v-if="$route.name == 'index'">
                <div @click="AnchorScroll('#showcase_anchor')" class="_sc cursor-pointer py-4">Showcase</div>
                <div @click="AnchorScroll('#fulllist_anchor')" class="_fl cursor-pointer py-4">Full list</div>
            </div>
            <a target="_blank" href="https://app.storyblok.com/#/me/spaces/181985/stories/0/0/index/0"> Edit </a>
        </div>
    </section>
</template>

<script>
import gsap from 'gsap'
import { ScrollToPlugin } from 'gsap/dist/ScrollToPlugin.min'

gsap.registerPlugin(ScrollToPlugin)

export default {
    data: () => ({
        is_showcase: true,
    }),
    methods: {
        AnchorScroll(elem) {
            gsap.to(window, { duration: 1, scrollTo: elem, ease: 'power2.inOut' })
        },
    },
}
</script>

<style lang="scss" scoped>
#header .__header {
    position: fixed;
    width: 100%;
    z-index: 10000;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0 calc(64rem / 16);
    font-weight: bold;

    height: calc(80rem / 16);
    background: var(--black);
    font-size: calc(24rem / 16);
    box-shadow: 0 0 20px #000;
    backdrop-filter: blur(5px);
    &.showcase {
        height: calc(120rem / 16);
        background: linear-gradient(0deg, #0004, #000b);
        font-size: calc(36rem / 16);
        box-shadow: 0 0 20px #000b;

        ._sc {
            text-decoration: underline;
        }
    }
    &:not(.showcase) {
        ._fl {
            text-decoration: underline;
        }
    }
    transition: height 0.3s ease-in-out, font-size 0.3s ease-in-out;
    @media (max-width: $phone) {
        &,
        &.showcase {
            padding: 0 calc(20rem / 16);
            height: calc(60rem / 16);
            font-size: calc(16rem / 16);
        }
    }
}
</style>
