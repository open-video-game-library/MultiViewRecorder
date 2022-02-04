<template>
    <div class="wrapper">
        <div class="container">
            <select id="select-canvasSize" v-model="canvasSize">
                <option v-for="option in size_options" :value="option.height" :key="option.width">
                    {{ option.width + ' * ' + option.height }}
                </option> 
            </select>
            <select id="select-canvasSize" v-model="canvasSize">
                <option v-for="option in size_options" :value="option.height" :key="option.width">
                    {{ option.width + ' * ' + option.height }}
                </option> 
            </select>
        </div>
    </div>
</template>

<script>
export default {
    name: 'App',
    components: {
    },
    computed: {
        canvasSize: {
            get() {
                return this.size_options[0].height
            },
            set(newValue) {
                this.$emit("changeCanvasSize", newValue)
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

<style scoped>
#select-canvasSize {
    width: 500px;
}
.wrapper {
    position: fixed;
    top: 0; bottom: 0; left: 0; right: 0;
    background-color: rgba(0, 0, 0, 0.2);
}
.container {
    position: fixed;
    top: 80px; bottom: 80px; left: 80px; right: 80px;
    border-radius: 8px;
    background-color: #fff;
}
</style>
