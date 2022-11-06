<template>
    <section class="py-16 relative" id="full_list_component">
        <div
            v-if="filtered_list && filtered_list.length == 0"
            class="absolute top-1/4 left-0 w-full text-center opacity-30 text-20"
        >
            No matches, empty match carton
        </div>
        <div class="grid grid-cols-4 gap-8 px-16">
            <FullListElem
                v-for="(item, i) in filtered_list || game_list"
                :key="i"
                class="bg-black h-60 relative overflow-hidden group"
                :item="item"
            >
            </FullListElem>
        </div>
    </section>
</template>

<script>
export default {
    data: () => ({
        filtered_list: null,

        searchText: '',
        sortBy: null,
        filters: null,
    }),
    props: ['game_list'],
    methods: {
        changeSearchText(newText) {
            this.searchText = newText
            this.runFilters()
        },

        runFilters() {
            this.filtered_list = []

            setTimeout(() => {
                this.filtered_list = [...this.game_list]

                if (this.searchText)
                    this.filtered_list = this.filtered_list.filter(
                        (elem) => elem.game.name.toLowerCase().search(this.searchText) != -1
                    )
            }, 100)
        },
    },
}
</script>

<style lang="scss" scoped>
#full_list_component {
    min-height: 100vh;
}
</style>
