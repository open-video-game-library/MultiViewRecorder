<template>
    <div class="wrapper">
        <div class="movie-wrapper">
            <CameraSelect :disabled="isRecording" @change-camera="changeCamera(0, $event)" />
            <CameraSelect :disabled="isRecording" @change-camera="changeCamera(1, $event)" />
            <canvas ref="canvas" :width="canvasWidth" :height="canvasHeight">Canvas not available.</canvas>
            <CameraSelect :disabled="isRecording" @change-camera="changeCamera(2, $event)" />
            <CameraSelect :disabled="isRecording" @change-camera="changeCamera(3, $event)" />
        </div>
        <section>
            <MicSelect :disabled="isRecording" @change-mic="changeMic" />
        </section>
        <section class="recording-info-wrapper">
            <button @click="clickRecordingBtn">{{ record_text }}</button>
            <p>Status : {{ record_status }}</p>
            <p>Time : {{ record_time }}</p>
        </section>
    </div>
</template>

<script>
import CameraSelect from './CameraSelect.vue'
import MicSelect from './MicSelect.vue'

export default {
    name: 'Recorder',
    components: {
        CameraSelect,
        MicSelect
    },
    props: {
        canvasHeight: Number,
        frameRate: Number,
        mimetype: String
    },
    mounted() {
        this.video.map((value, index) => { this.video[index].element = document.createElement('video') })
        this.canvas = this.$refs.canvas
        this.context = this.canvas.getContext("2d")
        // this.context.fillStyle = '#f2f2f2'
        // this.context.fillRect(0, 0, this.canvasWidth, this.canvasHeight)
    },
    methods: {
        changeCamera(videoIndex, value) {
            this.stopVideo(videoIndex);
            switch(videoIndex) {
                case 1:
                    this.video[videoIndex].posX = this.canvasWidth / 2
                    break
                case 2:
                    this.video[videoIndex].posY = this.canvasHeight / 2
                    break
                case 3:
                    this.video[videoIndex].posX = this.canvasWidth / 2
                    this.video[videoIndex].posY = this.canvasHeight / 2
                    break
            }
            switch(value) {
                case 'none':
                    break;
                case 'audioTime':
                    navigator.mediaDevices.getUserMedia({
                        audio: {
                            deviceId: this.audioId
                        },
                        video: false
                    }).then(stream => {
                        this.video[videoIndex].value = 'audioWaveform'
                        this.playAudioTime(videoIndex, stream)
                    }).catch(function(e) {
                        alert("ERROR: 音声の取得に失敗しました: " + e.message)
                    });
                    break;
                case 'audioFrequency':
                    navigator.mediaDevices.getUserMedia({
                        audio: {
                            deviceId: this.audioId
                        },
                        video: false
                    }).then(stream => {
                        this.video[videoIndex].value = 'audioWaveform'
                        this.playAudioFrequency(videoIndex, stream)
                    }).catch(function(e) {
                        alert("ERROR: 音声の取得に失敗しました: " + e.message)
                    });
                    break;
                case 'displayMedia':
                    navigator.mediaDevices.getDisplayMedia({
                        video: {
                            cursor: "always"
                        },
                        audio: true
                    }).then(stream => {
                        this.video[videoIndex].value = 'displayMedia'
                        this.playVideo(videoIndex, stream)
                    }).catch(function(e) {
                        alert("ERROR: ディスプレイのキャプチャに失敗しました: " + e.message)
                    });
                    break;
                default:
                    navigator.mediaDevices.getUserMedia({
                        audio: false,
                        video: {
                            width: this.canvasWidth / 2,
                            height: this.canvasHeight / 2,
                            deviceId: value,
                        }
                    }).then(stream => {
                        this.video[videoIndex].value = value
                        this.playVideo(videoIndex, stream)
                    }).catch(function(e) {
                        alert("ERROR: Webカメラの起動に失敗しました: " + e.message);
                    });
            }

        },
        changeMic(value) {
            if (value === 'none') this.audioId = ''
            else this.audioId = value
            console.log('this.audioId: ' + this.audioId)
        },
        playVideo(videoIndex, stream) {
            this.video[videoIndex].element.srcObject = stream;
            this.video[videoIndex].element.onloadedmetadata = () => {
                // console.log('onloadedmetadata: ' + event + ', ' + videoIndex + ', ' + stream);
                this.video[videoIndex].element.play();
                this.video[videoIndex].timer = setInterval(() => {
                    if (this.context)
                        this.context.drawImage(this.video[videoIndex].element,
                            this.video[videoIndex].posX, this.video[videoIndex].posY,
                            this.canvasWidth / 2, this.canvasHeight / 2);
                }, 1000 / this.fps);
            };
        },
        playAudioTime(videoIndex, stream) {
            const audioContext = new AudioContext()
            const sourceNode = audioContext.createMediaStreamSource(stream)
            const analyserNode = audioContext.createAnalyser()
            sourceNode.connect(analyserNode)
            analyserNode.fftSize = 2048
            const array = new Uint8Array(analyserNode.fftSize)
            this.video[videoIndex].timer_audio = setInterval(() => {
                this.context.fillStyle = '#fefefe'
                this.context.fillRect(this.video[videoIndex].posX, this.video[videoIndex].posY, this.canvasWidth / 2, this.canvasHeight / 2)
                analyserNode.getByteTimeDomainData(array)
                const barWidth = (this.canvasWidth / 2) / analyserNode.fftSize
                for (let i = 0; i < analyserNode.fftSize; ++i) {
                    const value = array[i]
                    const percent = value / 255
                    const height = (this.canvasHeight / 2) * percent
                    const offset = (this.canvasHeight / 2) - height

                    this.context.fillStyle = 'rgb(50, ' + (offset + 100) + ', 50)'
                    this.context.fillRect(this.video[videoIndex].posX + (i * barWidth), this.video[videoIndex].posY + offset, barWidth, 2)
                }
            }, 1000 / this.fps);
        },
        playAudioFrequency(videoIndex, stream) {
            const audioContext = new AudioContext()
            const sourceNode = audioContext.createMediaStreamSource(stream)
            const analyserNode = audioContext.createAnalyser()
            sourceNode.connect(analyserNode)
            analyserNode.fftSize = 32768
            const bufferLength = analyserNode.frequencyBinCount
            const array = new Uint8Array(bufferLength)
            // 取得する周波数の範囲を設定
            const maxHzRange = 4000;
            const minHzRange = 200;
            const maxArrayIndex = parseInt((maxHzRange * analyserNode.fftSize) / 44100)
            const minArrayIndex = parseInt((minHzRange * analyserNode.fftSize) / 44100)
            console.log(`bufferLength: ${bufferLength}, maxArrayIndex: ${maxArrayIndex}, minArrayIndex: ${minArrayIndex}`)
            // 範囲内の新しい配列を生成
            const newArray = array.slice(minArrayIndex, maxArrayIndex)
            this.video[videoIndex].timer_audio = setInterval(() => {
                this.context.fillStyle = '#fefefe'
                this.context.fillRect(this.video[videoIndex].posX, this.video[videoIndex].posY, this.canvasWidth / 2, this.canvasHeight / 2)
                analyserNode.getByteFrequencyData(newArray)
                // const barWidth = (this.canvasWidth / 2) / bufferLength
                const barWidth = (this.canvasWidth / 2) / newArray.length
                let barHeight;
                let x = 0;
                for(let i = 0; i < newArray.length; i++) {
                    barHeight = newArray[i]
                    this.context.fillStyle = 'rgb(' + (barHeight + 100) + ', 50, 50)'
                    this.context.fillRect(this.video[videoIndex].posX + x, this.video[videoIndex].posY + (this.canvasHeight / 2) - barHeight / 2, barWidth, barHeight / 2)
                    x += barWidth
                }
                const maxHzIndex = newArray.indexOf(Math.max(...newArray))
                this.context.fillStyle = 'rgb(50 , 50, 50)'
                this.context.font = '48px sans-serif'
                this.context.fillText(Math.floor(maxHzIndex * (44100 / analyserNode.fftSize) * 1000) / 1000 + 'Hz', this.video[videoIndex].posX, this.video[videoIndex].posY + 48)
            }, 1000 / this.fps);
        },
        stopVideo(videoIndex) {
            switch(this.video[videoIndex].value) {
                case 'none':
                    break;
                case 'audioWaveform':
                    clearInterval(this.video[videoIndex].timer_audio)
                    break;
                case 'displayMedia':
                default:
                    if (this.video[videoIndex].element.srcObject !== null) {
                        this.video[videoIndex].element.pause()
                        this.video[videoIndex].element.srcObject.getTracks().forEach(track => { track.stop() })
                        this.video[videoIndex].element.srcObject = null
                    }
                    clearInterval(this.video[videoIndex].timer)
            }
            this.video[videoIndex].value = 'none'
            // this.context.fillStyle = '#f2f2f2'
            // this.context.fillRect(this.video[videoIndex].posX, this.video[videoIndex].posY, this.canvasWidth / 2, this.canvasHeight / 2)
            this.context.clearRect(this.video[videoIndex].posX, this.video[videoIndex].posY, this.canvasWidth / 2, this.canvasHeight / 2)
        },
        clickRecordingBtn() {
            if(!this.isRecording) {
                if (this.audioId === '') {
                    alert('マイクを選択してください')
                    return
                }
                if (!confirm('録画を開始します')) return;
                const videoStream = this.canvas.captureStream()
                navigator.mediaDevices.getUserMedia({
                    video: false,
                    audio: { deviceId: this.audioId }
                }).then(stream => {
                    const combinedStream = new MediaStream([videoStream.getTracks()[0], stream.getAudioTracks()[0]])
                    this.mediaRecorder = new MediaRecorder(combinedStream, { mimeType: this.mimetype })
                    if (this.mediaRecorder.state == "recording") return
                    this.record_status = this.mediaRecorder.state
                    this.timer_recording = setInterval(() => {
                        this.record_status = this.mediaRecorder.state
                        this.record_time = this.transSecToTime(this.time_recording_sec)
                        this.time_recording_sec++
                    }, 1000);
                    this.mediaRecorder.start()
                    this.isRecording = true
                    this.record_text = 'Stop recording'
                });
            } else {
                if (this.mediaRecorder.state == "inactive") return
                this.mediaRecorder.stop()
                this.isRecording = false

                //////// ダウンロード表示 ////////
                this.mediaRecorder.addEventListener('dataavailable', (event) => {
                    const anchor = document.createElement('a')
                    const videoBlob = new Blob([event.data], { type: event.data.type })
                    const blobUrl = window.URL.createObjectURL(videoBlob)
                    if (this.mimetype.match(/webm/)) anchor.download = this.getNow() + '.webm'
                    if (this.mimetype.match(/WEBM/)) anchor.download = this.getNow() + '.WEBM'
                    else if (this.mimetype.match(/matroska/)) anchor.download = this.getNow() + '.mkv'
                    else if (this.mimetype.match(/mpeg/)) anchor.download = this.getNow() + '.mp4'
                    anchor.href = blobUrl
                    anchor.click()
                    clearInterval(this.timer_recording)
                    this.record_status = this.mediaRecorder.state
                    this.time_recording_sec = 0
                    this.record_text = 'Start recording'
                });
            }
        },
        getNow() {
            const now = new Date()
            const y = now.getFullYear()
            const m = ('00' + now.getMonth()).slice(-2)
            const d = ('00' + now.getDate()).slice(-2)
            const h = ('00' + now.getHours()).slice(-2)
            const min = ('00' + now.getMinutes()).slice(-2)
            const s = ('00' + now.getSeconds()).slice(-2)
            return y + m + d + '_' + h + min + s
        },
        transSecToTime(sec) {
            const h = ('00' + parseInt(sec / 3600)).slice(-2)
            const m = ('00' + parseInt(sec / 60)).slice(-2)
            const s = ('00' + (sec - ((h * 3600) + (m * 60)))).slice(-2)
            return h + ':' + m + ':' + s
        }
    },
    watch: {
        canvasHeight() {
            this.canvasWidth = (this.canvasHeight / 9) * 16
            this.selectWidth = (this.canvasWidth / 2) + 'px'
            this.canvas_height = this.canvasHeight + 'px'

            // 動画などのリサイズ
            this.video[1].posX = this.canvasWidth / 2
            this.video[2].posY = this.canvasHeight / 2
            this.video[3].posX = this.canvasWidth / 2
            this.video[3].posY = this.canvasHeight / 2
        },
        frameRate() {
            console.log(this.frameRate)
            this.fps = this.frameRate
        },
        mimetype() {

        }
    },
    data() {
        return {
            canvasWidth: 640,
            selectWidth: '320px',
            canvas_height: '360px',
            video: [
                {
                    value: 'none',
                    element: null,
                    posX: 0,
                    posY: 0,
                    timer: null,
                    timer_audio: null
                },
                {
                    value: 'none',
                    element: null,
                    posX: this.canvasWidth / 2,
                    posY: 0,
                    timer: null,
                    timer_audio: null
                },
                {
                    value: 'none',
                    element: null,
                    posX: 0,
                    posY: 0,
                    timer: null,
                    timer_audio: null
                },
                {
                    value: 'none',
                    element: null,
                    posX: this.canvasWidth / 2,
                    posY: this.canvasHeight / 2,
                    timer: null,
                    timer_audio: null
                }
            ],
            audioId: '',
            fps: 30,
            isRecording: false,
            record_text: 'Start recording',
            record_status: 'inactive',
            record_time: '00:00:00',
            time_recording_sec: 0,
            timer_recording: null
        }
    }
}
</script>

<style lang="scss" scoped>
.movie-wrapper {
    display: grid;
    grid-template-columns: v-bind(selectWidth) v-bind(selectWidth);
    grid-template-rows: 26px v-bind(canvas_height) 26px;
    canvas {
        grid-column: 1 / 3;
        border: #202020 solid 1px;
    }
}
.recording-info-wrapper {
    width: 640px;
    margin: 24px auto 0;
    display: flex;
    justify-content: space-between;
    * { flex-basis: 33%; }
    button {
        background-color: white;
        color: red;
        border-color: red;
        border-radius: 2px;
    }
}
section {
    margin-top: 24px;
}
</style>
