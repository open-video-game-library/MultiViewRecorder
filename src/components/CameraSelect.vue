<template>
    <select v-model="changeCamera">
        <option v-for="option in camera_options" :value="option.value" :key="option.value">
            {{ option.name }}
        </option>
    </select>
</template>

<script>
export default {
    name: 'CameraSelect',
    components: {
        
    },
    mounted() {
        (async () => {
            await navigator.mediaDevices.getUserMedia({ audio: true, video: true });
            const devices = await navigator.mediaDevices.enumerateDevices();
            devices.map(device => {
                if (device.kind === "videoinput")
                    this.camera_options.push({
                        name: device.label,
                        value: device.deviceId
                    });
            });
        })().then(() => {
            this.camera_options[0].name = 'Select a camera.'
        });
    },
    computed: {
        changeCamera: {
            get() {
                return this.camera_options[0].value
            },
            set(newValue) {
                this.$emit("change-camera", newValue)
            },
        },
    },
    methods: {
    },
    data() {
        return {
            camera_options: [
                {
                    value: 'none',
                    name: 'Loading...'
                },
                {
                    value: 'displayMedia',
                    name: 'Display'
                },
                {
                    value: 'audioTime',
                    name: 'Audio Time Domain'
                },
                {
                    value: 'audioFrequency',
                    name: 'Audio Frequency Domain'
                }
            ]
        }
    }
}
</script>

<style>
</style>
