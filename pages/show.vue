<template>
    <main class="grid grid-cols-1">
        <div id="showcase_anchor"></div>
        <Favs :fav_list="fav_list" />
        <HeaderSwitch />
        <div id="fulllist_anchor"></div>
        <FullList :game_list="[...game_list, ...game_list, ...game_list, ...game_list]" />
    </main>
</template>

<script>
export default {
    data: () => ({
        fav_list: [
            {
                id: 1,
                title: 'Elden Ring',
                front_img: require('~/assets/img/placeholders/elden_ring_front.webp'),
                back_img: require('~/assets/img/placeholders/elden_ring_back.jpg'),
                show_date: 0,
                list: [
                    {
                        console: 'ps5',
                        date: 1672012800,
                    },
                ],
            },
            {
                id: 2,
                title: 'Metal Gear Solid 5',
                front_img: require('~/assets/img/placeholders/mgs5_front.jpg'),
                back_img: require('~/assets/img/placeholders/mgs5_back.jpg'),
                show_date: 0,
                list: [
                    {
                        console: 'ps5',
                        date: 1649716000,
                    },
                    {
                        console: 'ps3',
                        date: 1649718000,
                    },
                ],
            },
            {
                id: 3,
                title: "Demon's Souls",
                front_img: require('~/assets/img/placeholders/demon_front.webp'),
                back_img: require('~/assets/img/placeholders/demon_back.png'),
                show_date: 0,
                list: [
                    {
                        console: 'ps5',
                        date: 1285887600,
                    },
                ],
            },
        ],
        game_list: [
            {
                id: 1,
                title: 'Elden Ring',
                front_img: require('~/assets/img/placeholders/elden_ring_front.webp'),
                back_img: require('~/assets/img/placeholders/elden_ring_back.jpg'),
                show_date: 0,
                list: [
                    {
                        console: 'ps5',
                        date: 1672012800,
                    },
                ],
            },
            {
                id: 2,
                title: 'Metal Gear Solid 5',
                front_img: require('~/assets/img/placeholders/mgs5_front.jpg'),
                back_img: require('~/assets/img/placeholders/mgs5_back.jpg'),
                show_date: 0,
                list: [
                    {
                        console: 'ps5',
                        date: 1648716000,
                    },
                    {
                        console: 'ps3',
                        date: 1649718000,
                    },
                ],
            },
            {
                id: 3,
                title: "Demon's Souls",
                front_img: require('~/assets/img/placeholders/demon_front.webp'),
                back_img: require('~/assets/img/placeholders/demon_back.png'),
                show_date: 0,
                list: [
                    {
                        console: 'ps5',
                        date: 1285887600,
                    },
                ],
            },
            {
                id: 4,
                title: 'Ghostrunner',
                front_img: 'https://thegeek.games/wp-content/uploads/2020/05/thegeek-ghostrunner-5.jpg',
                show_date: 0,
                list: [
                    {
                        console: 'st',
                        date: 1672012800,
                    },
                ],
            },
            {
                id: 5,
                title: 'UnMetal',
                front_img: 'https://image.api.playstation.com/vulcan/ap/rnd/202109/0121/1ieFmN8ZC88SokfHIZFQTe2L.jpg',
                show_date: 0,
                list: [
                    {
                        console: 'ps5',
                        date: 1285887600,
                    },
                ],
            },
        ],
    }),
}
</script>

<style lang="scss" scoped></style>
