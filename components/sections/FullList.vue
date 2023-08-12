<template>
    <section class="py-10 md:py-16 relative __1920" id="full_list_component">
        <div
            v-if="filtered_list && filtered_list.length == 0"
            class="absolute top-1/2 left-0 w-full text-center opacity-30 text-20"
        >
            {{ loading ? 'Loading...' : 'No matches, empty match carton' }}
        </div>
        <div class="grid md:grid-cols-4 gap-8 px-4 md:px-16">
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
const SORT_ASC = (a, b) => a.content.Date > b.content.Date
const SORT_DESC = (a, b) => a.content.Date < b.content.Date

export default {
    data: () => ({
        filtered_list: null,

        searchText: '',
        sortBy: null,
        filters: null,

        order_asc: false,

        loading: false,
    }),
    props: ['game_list'],
    methods: {
        changeSearchText(newText) {
            this.searchText = newText
            this.runFilters()
        },
        changeSortDir(asc) {
            this.order_asc = asc
            this.runFilters()
        },

        runFilters() {
            this.loading = true
            this.filtered_list = []

            setTimeout(() => {
                this.filtered_list = [...this.game_list]
                this.filtered_list.sort(this.order_asc ? SORT_ASC : SORT_DESC)

                if (this.searchText)
                    this.filtered_list = this.filtered_list.filter(
                        (elem) => elem.game.name.toLowerCase().search(this.searchText) != -1
                    )
                this.loading = false
            }, 100)
        },
    },
    mounted() {
        this.runFilters()
    },
}
</script>

<style lang="scss" scoped>
#full_list_component {
    min-height: 60vh;
}
</style>
