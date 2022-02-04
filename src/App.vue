<template>
    <div>
        <header>
            <h1>Multi View Recorder</h1>
        </header>
        <Settings @changeCanvasSize="setCanvasSize" />
        <main>
            <Recorder id="recorder" :canvasHeight="canvasHeight" />
        </main>
        <footer></footer>
    </div>
</template>

<script>
import 'normalize.css'
import Recorder from './components/Recorder.vue'
import Settings from './components/Settings.vue'

export default {
    name: 'App',
    components: {
        Recorder,
        Settings
    },
    methods: {
        setCanvasSize(value) {
            this.canvasHeight = value
        }
    },
    data(){
        return {
            canvasHeight: 360
        }
    }
}
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}
#recorder {
    margin: 0 auto;
}
</style>
