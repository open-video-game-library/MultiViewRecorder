<template>
    <select v-model="changeMic">
        <option v-for="option in mic_options" :value="option.value" :key="option.value">
            {{ option.name }}
        </option>
    </select>
</template>

<script>
export default {
    name: 'MicSelect',
    components: {
        
    },
    mounted() {
        (async () => {
            await navigator.mediaDevices.getUserMedia({ audio: true, video: true });
            const devices = await navigator.mediaDevices.enumerateDevices();
            devices.map(device => {
                if (device.kind === "audioinput")
                    this.mic_options.push({
                        name: device.label,
                        value: device.deviceId
                    });
            });
        })().then(() => {
            this.mic_options[0].name = 'Select a microphone.'
        });
    },
    computed: {
        changeMic: {
            get() {
                return this.mic_options[0].value
            },
            set(newValue) {
                this.$emit("change-mic", newValue)
            },
        },
    },
    methods: {
    },
    data() {
        return {
            mic_options: [
                {
                    value: 'none',
                    name: 'Loading...'
                }
            ]
        }
    }
}
</script>

<style>
</style>
