<template>
    <main>
        <section class="w-screen h-screen flex justify-center items-center text-8 py-4">
            Error: {{ error.statusCode }}
        </section>
    </main>
</template>

<script>
export default {
    props: ['error'],
}
</script>

<style scoped lang="scss"></style>
