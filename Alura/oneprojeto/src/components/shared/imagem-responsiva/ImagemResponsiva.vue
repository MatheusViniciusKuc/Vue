<!-- eslint-disable vue/valid-template-root -->
<template>
    <img class="imagem-responsiva" :src="url" :alt="titulo">
</template>

<script>
export default {

    props: {
        url: {
            type: String,
            required: true
        },

        titulo: {
            type: String,
            required: true
        }
    }

}
</script>

<style scoped>

    img {
        width: 100%;
    }

</style>