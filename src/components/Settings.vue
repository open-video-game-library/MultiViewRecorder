<template>
    <div class="background-shadow">
        <div class="popup">
            <header class="wrapper">
                <div class="container">
                    <h2>Settings</h2>
                    <p id="btn-icon-times" class="btn-icon" @click="closeSettings">
                        <fa icon="times" />
                    </p>
                </div>
            </header>
            <select id="select-canvasSize" v-model="canvasSize">
                <option v-for="option in size_options" :value="option.height" :key="option.width">
                    {{ option.width + ' * ' + option.height }}
                </option> 
            </select>
            <!-- <select id="select-canvasSize" v-model="canvasSize">
                <option v-for="option in size_options" :value="option.height" :key="option.width">
                    {{ option.width + ' * ' + option.height }}
                </option> 
            </select> -->
        </div>
    </div>
</template>

<script>
export default {
    name: 'App',
    components: {
    },
    methods: {
        closeSettings() {
            this.$emit('close-settings')
        }
    },
    props: {
        canvasHeight: Number
    },
    computed: {
        canvasSize: {
            get() {
                return this.canvasHeight
            },
            set(newValue) {
                this.$emit('change-canvas-size', newValue)
            },
        },
    },
    data() {
        return {
            size_options: [
                {
                    width: 640,
                    height: 360
                },
                {
                    width: 1280,
                    height: 720
                },
                {
                    width: 1920,
                    height: 1080
                }
            ],
            mimetype_options: [
                {
                    type: "video/webm"
                },
                {
                    type: "video/webm\\;codecs=vp8"
                },
                {
                    type: "video/webm\\;codecs=daala"
                },
                {
                    type: "video/webm\\;codecs=h264"
                },
                {
                    type: "video/mpeg"
                }
            ]
        }
    }
}
</script>

<style lang="scss">
.background-shadow {
    position: fixed;
    top: 0; bottom: 0; left: 0; right: 0;
    background-color: rgba(0, 0, 0, 0.2);
}
.popup {
    position: fixed;
    top: 80px; bottom: 80px; left: 80px; right: 80px;
    border-radius: 8px;
    background-color: #fff;
}
</style>

<style lang="scss" scoped>
#select-canvasSize {
    width: 500px;
}
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
