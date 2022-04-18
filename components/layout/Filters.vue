<template>
    <section class="pt-24 pb-8 px-16">
        <div class="flex justify-between">
            <div class=""></div>
            <div class="flex items-center">
                <input
                    type="text"
                    class="_search_bar"
                    :class="is_search_open ? '' : '_hidden'"
                    @input="onSearchInput()"
                />
                <div class="cursor-pointer p-4" @click="onSearchClick()">
                    <i class="fas fa-search"></i>
                </div>
            </div>
        </div>
    </section>
</template>

<script>
export default {
    data: () => ({
        is_search_open: false,
        search_bar_countdown: null,

        full_list_component: null,
    }),
    methods: {
        onSearchClick() {
            this.is_search_open = !this.is_search_open
        },
        onSearchInput() {
            if (this.search_bar_countdown) clearTimeout(this.search_bar_countdown)

            this.search_bar_countdown = setTimeout(() => {
                if (!this.full_list_component) return
                this.full_list_component.changeSearchText(this.$el.querySelector('input._search_bar').value)
            }, 500)
        },
    },
    mounted() {
        this.full_list_component = document.querySelector('#full_list_component')
        this.full_list_component = this.full_list_component ? this.full_list_component.__vue__ : null
    },
}
</script>

<style lang="scss" scoped>
input._search_bar {
    color: var(--black);
    font-weight: bold;
    padding: calc(8rem / 16) calc(12rem / 16);
    margin-right: calc(16rem / 16);
    border: unset;
    outline: none;
    box-shadow: 0 0 calc(10rem / 16) var(--blue);
    width: calc(420rem / 16);

    overflow: hidden;
    transition: width 0.3s, opacity 0.3s, visibility 0.3s;

    &._hidden {
        width: 0px;
        opacity: 0;
        visibility: hidden;
    }
}
</style>
