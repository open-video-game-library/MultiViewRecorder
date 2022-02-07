<template>
    <div>
        <header class="wrapper">
            <div class="container">
                <h1>Multi View Recorder</h1>
                <div class="btns">
                    <p class="btn-icon" @click="openInformation"><fa icon="question-circle" /></p>
                    <p class="btn-icon" @click="openSettings"><fa icon="cog" /></p>
                </div>
            </div>
        </header>
        <transition name="fade">
            <Settings v-show="showSettings"
                @change-canvas-size="setCanvasSize"
                @close-settings="closeSettings"
                :canvasHeight="canvasHeight"
            />
        </transition>
        <transition name="fade">
            <Information v-show="showInformation"
                @close-information="closeInformation"
            />
        </transition>
        <main class="wrapper">
            <div class="container">
                <Recorder id="recorder" :canvasHeight="canvasHeight" />
            </div>
        </main>
        <footer>
            Presented by Open Video Game Library <fa icon="copyright" /> {{ year }} WATANABE LABORATORY, Meiji Univ.
        </footer>
    </div>
</template>

<script>
import Recorder from './components/Recorder.vue'
import Settings from './components/Settings.vue'
import Information from './components/Information.vue'

export default {
    name: 'App',
    components: {
        Recorder,
        Settings,
        Information
    },
    mounted() {
        this.year = new Date().getFullYear()
    },
    methods: {
        setCanvasSize(value) {
            this.canvasHeight = value
        },
        openSettings() {
            this.showSettings = true
        },
        closeSettings() {
            this.showSettings = false
        },
        openInformation() {
            this.showInformation = true
        },
        closeInformation() {
            this.showInformation = false
        },
    },
    data(){
        return {
            canvasHeight: 360,
            showSettings: false,
            showInformation: false,
            year: 2022
        }
    }
}
</script>

<style lang="scss">
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
}
.btn-icon {
    display: inline-block;
    font-size: 24px;
    width: 24px; height: 24px;
    line-height: 24px;
    :hover {
        cursor: pointer;
    }
}
#recorder {
    text-align: center;
    margin: 0 auto;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>

<style lang="scss" scoped>
.wrapper {
    .container {
        max-width: 1280px;
        margin: 0 auto;
        padding: 0 24px;
    }
}
header .container {
    display: flex;
    justify-content: space-between;
    h1 {
        margin: 24px 0;
    }
    .btns p {
        margin-left: 24px;
    }
}
main {
    display: flex;
    justify-content: center;
    background-color: #F8F9FA;
    padding: 48px 0;
}
footer {
    padding: 48px 0;
}
</style>