<template>
    <section>
        <div class="w-full h-screen relative overflow-hidden">
            <div v-for="(item, i) in fav_list" :key="i" class="_slide bg-black absolute w-full h-full top-0 left-0">
                <div class="_img w-full h-full">
                    <Photo :src_dsk="item.back_img || item.front_img" class="w-full h-full" />
                </div>
                <div class="absolute w-full h-full top-0 left-0 bg-black opacity-40"></div>
                <div class="absolute top-1/2 -translate-y-1/2" :class="i % 2 ? 'right-20 text-right' : 'left-20'">
                    <div class="_title text-20 font-bold">{{ item.title }}</div>
                    <div class="_console flex gap-4 text-8 pb-6" :class="i % 2 ? 'justify-end' : ''">
                        <ConsoleDict type="logo" :code="item.list[0].console" />
                        <ConsoleDict type="name" :code="item.list[0].console" />
                    </div>
                    <div class="_date text-6">{{ timestampToDate(item.list[0].date) }}</div>
                </div>
            </div>
        </div>
    </section>
</template>

<script>
import gsap from 'gsap'
import { ScrollToPlugin } from 'gsap/dist/ScrollToPlugin.min'

gsap.registerPlugin(ScrollToPlugin)

export default {
    props: ['fav_list'],
    data: () => ({
        timeline: null,
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
            let trans_time = 1
            let slide_time = 6

            this.timeline = gsap.timeline({ repeat: -1 })

            let slides = this.$el.querySelectorAll('._slide')

            for (let i = 0; i < slides.length; i += 1) {
                let slide = slides[i]
                let img = slide.querySelector('._img')
                let texts = [
                    slide.querySelector('._title'),
                    slide.querySelector('._console'),
                    slide.querySelector('._date'),
                ]

                this.timeline.fromTo(slide, { opacity: 0 }, { opacity: 1, duration: trans_time, ease: 'power2.inOut' })
                this.timeline.to(slide, { opacity: 0, duration: trans_time, ease: 'power2.inOut' }, `+=${slide_time}`)

                let anim = this.getRandomImgAnim()

                this.timeline.fromTo(
                    img,
                    Object.assign({ scale: 1.1 }, anim.in),
                    Object.assign({ scale: 1.1, duration: 2 * trans_time + slide_time, ease: 'linear' }, anim.out),
                    `-=${2 * trans_time + slide_time}`
                )

                this.timeline.add(() => {}, `-=${trans_time + slide_time}`)

                for (let t = 0; t < texts.length; t += 1) {
                    this.timeline.fromTo(
                        texts[t],
                        { opacity: 0 },
                        { opacity: 1, duration: slide_time / (texts.length * 2), ease: 'power2.inOut' },
                        `>`
                    )
                }
            }

            // console.log(slides)
        },

        getRandomImgAnim() {
            let anims = [
                { in: { x: '4%' }, out: { x: '-4%' } },
                { in: { x: '-4%' }, out: { x: '4%' } },
                { in: { x: '4%', y: '2%' }, out: { x: '-4%', y: '-2%' } },
                { in: { x: '-4%', y: '2%' }, out: { x: '4%', y: '-2%' } },
                { in: { x: '4%', y: '-2%' }, out: { x: '-4%', y: '2%' } },
                { in: { x: '-4%', y: '-2%' }, out: { x: '4%', y: '2%' } },
            ]

            return anims[Math.floor(Math.random() * anims.length)]
        },
    },
    mounted() {
        this.InitGSAP()
    },
}
</script>

<style lang="scss" scoped></style>
