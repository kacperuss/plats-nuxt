<template>
    <section id="header">
        <div class="__header" :class="is_showcase ? 'showcase' : ''">
            <a href="/">
                <i class="fas fa-trophy"></i>
                KP
            </a>
            <div class="flex justify-between items-center gap-12">
                <div @click="AnchorScroll('#showcase_anchor')" class="_sc cursor-pointer">Showcase</div>
                <div @click="AnchorScroll('#fulllist_anchor')" class="_fl cursor-pointer">Full list</div>
            </div>
            <a href="/"> Log in </a>
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
    transition: height 0.3s, font-size 0.3s;
}
</style>
