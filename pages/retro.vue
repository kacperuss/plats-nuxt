<template>
    <main class="p-4 md:p-10">
        <div class="pt-24"></div>
        <div class="text-center px-6">
            <div class="text-12">Kacperuss Retro Viewer</div>
            <div class="text-6">A RetroAchievements Progress Viewer</div>
            <div class="_buttons">
                <a target="_blank" href="https://retroachievements.org/user/Kacperuss"> Open RA User Profile </a>
                <a v-if="gameData" target="_blank" :href="`https://retroachievements.org/game/${gameData.id}`">
                    Open RA Game Page
                </a>
            </div>
        </div>
        <div class="pt-16"></div>
        <div class="md:px-40" v-if="gameData">
            <div class="pb-12">
                <div
                    class="md:flex justify-between items-center"
                    :class="{
                        __platted: gameData.numAwardedToUserHardcore == gameData.numAchievements,
                    }"
                >
                    <div class="text-center md:text-left text-8 md:text-12 pb-6 md:pb-0">{{ gameData.title }}</div>
                    <div class="text-6 md:text-8">
                        <div class="text-right pb-2">
                            {{ getPercent(gameData.numAwardedToUserHardcore / gameData.numAchievements) }} ({{
                                gameData.numAwardedToUserHardcore
                            }}/{{ gameData.numAchievements }})
                        </div>
                        <div class="w-full md:w-110 h-[8px] bg-[#000] border border-current rounded">
                            <div
                                class="h-full bg-current"
                                :style="`width: ${getPercent(
                                    gameData.numAwardedToUserHardcore / gameData.numAchievements
                                )}`"
                            ></div>
                        </div>
                    </div>
                </div>
                <a href="/retro/" class="hover:opacity-50 pt-4 block">
                    <i class="fa-solid fa-caret-left"></i> Back to all games
                </a>
            </div>
            <div class="grid gap-16 md:gap-4">
                <div
                    class="__achic"
                    v-for="achic in gameData.achievements"
                    :class="{
                        __unlocked: !!achic.dateEarnedHardcore,
                    }"
                >
                    <img :src="`https://media.retroachievements.org/Badge/${achic.badgeName}.png`" alt="" />
                    <div class="">
                        <div class="text-6 md:text-8">{{ achic.title }}</div>
                        <div class="">{{ achic.description }}</div>
                    </div>
                    <div class="col-span-full md:col-span-1">
                        <div class="text-right pb-2">
                            {{ getPercent(achic.numAwarded / gameData.players_total) }}
                        </div>
                        <div class="w-full h-[8px] bg-[#000] opacity-50 border border-current rounded">
                            <div
                                class="h-full bg-current"
                                :style="`width: ${getPercent(achic.numAwarded / gameData.players_total)}`"
                            ></div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="md:px-40" v-if="allGames">
            <div class="grid gap-16 md:gap-4">
                <div class="" v-for="game in allGames">
                    <a
                        :href="`/retro/?game=${game.gameId}`"
                        class="__game"
                        :class="{
                            __platted: game.numAchieved == game.numPossibleAchievements,
                        }"
                    >
                        <img :src="`https://media.retroachievements.org/${game.imageIcon}`" alt="" />
                        <div class="">
                            <div class="text-6 md:text-8">{{ game.title }}</div>
                            <div class="">{{ game.consoleName }}</div>
                        </div>
                        <div class="col-span-full md:col-span-1">
                            <div class="text-right pb-2">
                                {{ getPercent(game.numAchieved / game.numPossibleAchievements) }}
                            </div>
                            <div class="w-full h-[8px] bg-[#000] opacity-50 border border-current rounded">
                                <div
                                    class="h-full bg-current"
                                    :style="`width: ${getPercent(game.numAchieved / game.numPossibleAchievements)}`"
                                ></div>
                            </div>
                        </div>
                    </a>
                </div>
            </div>
        </div>
    </main>
</template>

<script>
import { buildAuthorization, getGameInfoAndUserProgress, getUserRecentlyPlayedGames } from '@retroachievements/api'

export default {
    data: () => ({
        gameData: null,
        allGames: null,
    }),
    methods: {
        async getGameData(auth, id) {
            const data = await getGameInfoAndUserProgress(auth, {
                userName: 'Kacperuss',
                gameId: id,
            })
            console.log(JSON.parse(JSON.stringify(data)))
            this.gameData = data
        },
        async getAllGames(auth) {
            const games = await getUserRecentlyPlayedGames(auth, {
                userName: 'Kacperuss',
            })
            console.log(JSON.parse(JSON.stringify(games)))
            this.allGames = games
        },

        getPercent(val) {
            return (val * 100).toFixed(2) + '%'
        },
    },
    mounted() {
        const userName = 'Kacperuss'
        const webApiKey = 'yWVq62d6axsSAVlssY5XlLjz0wOnTMle'
        const auth = buildAuthorization({ userName, webApiKey })

        const game = this.$route.query?.game
        if (game) this.getGameData(auth, game)
        else this.getAllGames(auth)
    },
}
</script>

<style lang="scss" scoped>
main {
    max-width: 1920px;
    margin: 0 auto;
}
._buttons {
    padding: 1em;
    display: flex;
    justify-content: center;
    gap: 2em;
    a {
        background: var(--black);
        padding: 0.7em 1.2em;
        border-radius: 1em;
        &:hover {
            opacity: 0.5;
        }
    }
    @media (max-width: $phone) {
        flex-direction: column;
    }
}
.__achic {
    // height: 64px;
    display: grid;
    grid-template-columns: 64px 2fr 1fr;
    grid-template-rows: 64px;
    align-items: center;
    gap: 4em;
    color: #fff8;
    img {
        filter: grayscale(1) contrast(0.5);
    }
    &.__unlocked {
        color: #80ce4c;
        img {
            filter: none;
        }
    }
    @media (max-width: $phone) {
        grid-template-columns: 64px 1fr;
        gap: 1em;
    }
}
.__game {
    display: grid;
    // height: 96px;
    grid-template-columns: 96px 2fr 1fr;
    grid-template-rows: 96px;
    align-items: center;
    gap: 4em;
    @media (max-width: $phone) {
        grid-template-columns: 96px 1fr;
        gap: 1em;
    }
}
.__platted {
    color: #80ce4c;
}
</style>
