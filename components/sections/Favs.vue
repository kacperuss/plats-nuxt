<template>
    <section class="select-none">
        <div class="w-full h-screen relative overflow-hidden">
            <div v-for="(item, i) in fav_list" :key="i" class="_slide bg-black absolute w-full h-full top-0 left-0">
                <div class="_img w-[177vh] md:w-full h-full">
                    <Photo :src_dsk="$getImg(item, false)" class="w-full h-full" />
                </div>
                <div class="absolute w-full h-full top-0 left-0 bg-black opacity-40"></div>
                <div
                    class="absolute top-1/2 -translate-y-1/2"
                    :class="i % 2 ? 'right-4 md:right-20 text-right' : 'left-4 md:left-20'"
                >
                    <div class="_title text-10 md:text-20 font-bold pb-4">{{ item.name }}</div>
                    <div class="">
                        <div
                            class="_console flex gap-4 text-6 md:text-8 pb-2"
                            :class="i % 2 ? 'flex-row-reverse' : ''"
                            v-for="(console, j) in item.plats"
                            :key="`console_${i}_${j}`"
                        >
                            <ConsoleDict type="logo" :code="console.Platform" />
                            <ConsoleDict type="name" :code="console.Platform" />
                        </div>
                    </div>
                    <div class="_date text-6" v-if="item.plats.length == 1">
                        {{ item.plats[0].Date.split(' ')[0] }}
                    </div>
                </div>
            </div>
        </div>
    </section>
</template>

<script>
import gsap from 'gsap'

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
                    ...slide.querySelectorAll('._console'),
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
                    if (!texts[t]) continue
                    this.timeline.fromTo(
                        texts[t],
                        { opacity: 0 },
                        { opacity: 1, duration: slide_time / 6, ease: 'power2.inOut' },
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
            let anims_mob = [
                { in: { x: '-20%' }, out: { x: '-30%' } },
                { in: { x: '-30%' }, out: { x: '-20%' } },
                { in: { x: '-60%' }, out: { x: '-70%' } },
                { in: { x: '-70%' }, out: { x: '-60%' } },
                { in: { x: '-40%', y: '2%' }, out: { x: '-50%', y: '-2%' } },
                { in: { x: '-40%', y: '-2%' }, out: { x: '-50%', y: '2%' } },
                { in: { x: '-50%', y: '-2%' }, out: { x: '-40%', y: '2%' } },
                { in: { x: '-50%', y: '2%' }, out: { x: '-40%', y: '-2%' } },
            ]
            let is_mobile = window.matchMedia(`(max-width: ${this.$getRootVar('--phone-w')})`).matches

            return is_mobile
                ? anims_mob[Math.floor(Math.random() * anims_mob.length)]
                : anims[Math.floor(Math.random() * anims.length)]
        },
    },
    mounted() {
        this.InitGSAP()
    },
}
</script>

<style lang="scss" scoped></style>
