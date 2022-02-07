<template>
    <div class="background-shadow">
        <div class="popup">
            <header class="wrapper">
                <div class="container">
                    <h2>How to use</h2>
                    <p id="btn-icon-times" class="btn-icon" @click="closeInformation">
                        <fa icon="times" />
                    </p>
                </div>
            </header>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Information',
    components: {
    },
    methods: {
        closeInformation() {
            this.$emit('close-information')
        }
    },
    data() {
        return {
        }
    }
}
</script>

<style lang="scss" scoped>
header {
    .container {
        display: flex;
        justify-content: space-between;
    }
}
.wrapper {
    .container {
        max-width: 1280px;
        margin: 0 auto;
        padding: 0 24px;
    }
}
</style>