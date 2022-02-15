<template>
    <div class="background-shadow" @click.self="closeSettings">
        <div class="popup">
            <header class="wrapper">
                <h2>Settings</h2>
                <p id="btn-icon-times" class="btn-icon" @click="closeSettings">
                    <fa icon="times" />
                </p>
            </header>
            <main class="wrapper">
                <section class="setting-elem">
                    <h3>Size : </h3>
                    <select id="select-canvasSize" v-model="select_canvasHeight">
                        <option v-for="option in size_options" :value="option.height" :key="option.width">
                            {{ option.width + ' * ' + option.height }}
                        </option> 
                    </select>
                </section>
                <section class="setting-elem">
                    <h3>Frame rate (canvas) : </h3>
                    <select id="select-frameRate" v-model="select_frameRate">
                        <option v-for="option in frame_options" :value="option" :key="option">
                            {{ option }}
                        </option> 
                    </select>
                </section>
                <section class="setting-elem">
                    <h3>Mimetype : </h3>
                    <select id="select-mimetype" v-model="select_mimetype">
                        <option v-for="option in mimetype_options" :value="option.name" :key="option.name" :disabled="!option.isSupported">
                            {{ option.name }}
                        </option> 
                    </select>
                </section>
            </main>
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
        canvasHeight: Number,
        frameRate: Number,
        mimetype: String
    },
    mounted() {
        this.mimetype_options.map((value, index) => {
            if(MediaRecorder.isTypeSupported(value.name)) {
                this.mimetype_options[index].isSupported = true
                // this.select_mimetype = value.name
            }
        })
    },
    computed: {
        select_canvasHeight: {
            get() {
                return this.canvasHeight
            },
            set(newValue) {
                this.$emit('change-canvas-size', newValue)
            }
        },
        select_frameRate: {
            get() {
                return this.frameRate
            },
            set(newValue) {
                this.$emit('change-frame-rate', newValue)
            }
        },
        select_mimetype: {
            get() {
                return this.mimetype
            },
            set(newValue) {
                this.$emit('change-mimetype', newValue)
            }
        }
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
            frame_options: [15, 30, 60],
            mimetype_options: [
                {
                    name: 'video/webm',
                    isSupported: false
                },
                {        
                    name: 'video/webm;codecs=vp8',
                    isSupported: false
                },
                {        
                    name: 'video/webm;codecs=vp9',
                    isSupported: false
                },
                {        
                    name: 'video/webm;codecs=vp8.0',
                    isSupported: false
                },
                {        
                    name: 'video/webm;codecs=vp9.0',
                    isSupported: false
                },                
                {
                    name: 'video/webm;codecs=daala',
                    isSupported: false
                },
                {
                    name: 'video/webm;codecs=h264',
                    isSupported: false
                },
                {
                    name: 'video/webm;codecs=H264',
                    isSupported: false
                },
                {
                    name: 'video/webm;codecs=avc1',
                    isSupported: false
                },
                {        
                    name: 'video/webm;codecs=vp8,opus',
                    isSupported: false
                },
                {        
                    name: 'video/WEBM;codecs=VP8,OPUS',
                    isSupported: false
                },
                {        
                    name: 'video/webm;codecs=vp9,opus',
                    isSupported: false
                },
                {        
                    name: 'video/webm;codecs=vp8,vp9,opus',
                    isSupported: false
                },
                {
                    name: 'video/webm;codecs=h264,opus',
                    isSupported: false
                },
                {
                    name: 'video/webm;codecs=h264,vp9,opus',
                    isSupported: false
                },
                {
                    name: 'video/x-matroska;codecs=avc1',
                    isSupported: false
                },
                {
                    name: 'video/mpeg',
                    isSupported: false
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
    display: flex;
    justify-content: center;
    align-items: center;
}
.popup {
    width: 640px;
    height: 360px;
    position: fixed;
    border-radius: 8px;
    background-color: #fff;
    padding-bottom: 24px;
    overflow: auto;
    header {
        position: -webkit-sticky; /* safari対応 */
        position: sticky;
        top: 0px;
        background-color: white;
    }
}
</style>

<style lang="scss" scoped>
header {
    display: flex;
    justify-content: space-between;
}
.setting-elem {
    padding: 0 20px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    h3 {
        text-align: left;
        flex-basis: 40%;
    }
    select { flex-basis: 60%; }
}
.wrapper {
    margin: 0 auto;
    padding: 0 24px;
}
</style>
