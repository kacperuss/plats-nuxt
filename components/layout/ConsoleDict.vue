<template>
    <div class="h-full">
        <div v-if="code && type == 'name'">{{ console_dict[code] || console_dict['default'] }}</div>
        <div
            v-if="code && (type == 'logo' || type == 'icon')"
            class="w-full h-full"
            :title="console_dict[code] || console_dict['default']"
        >
            <i :class="console_icons[code] || console_icons['default']"></i>
        </div>
    </div>
</template>

<script>
export default {
    props: ['type', 'code'],
    data: () => ({
        console_dict: {
            ps5: 'PlayStation 5',
            ps4: 'PlayStation 4',
            ps3: 'PlayStation 3',
            psx: 'PlayStation+ Emulation',
            psra: 'PlayStation RetroAchievements',
            ps: 'Other PlayStation',
            st: 'Steam',
            gog: 'GOG',
            pc: 'PC',
            x360: 'Xbox 360',
            x1: 'Xbox One',
            xsx: 'Xbox Series X/S',
            x: 'Other Xbox',
            sw: 'Switch',
            default: 'Other / N/A',
        },
        console_icons: {
            ps5: 'fab fa-playstation',
            ps4: 'fab fa-playstation',
            ps3: 'fab fa-playstation',
            psx: 'fab fa-playstation',
            psra: 'fab fa-playstation',
            ps: 'fab fa-playstation',
            st: 'fab fa-steam',
            gog: 'fas fa-desktop',
            pc: 'fas fa-desktop',
            x360: 'fab fa-xbox',
            x1: 'fab fa-xbox',
            xsx: 'fab fa-xbox',
            sw: 'fas fa-tablet-alt',
            default: 'fas fa-gamepad',
        },
    }),
    methods: {},
}
</script>

<style lang="scss" scoped></style>
