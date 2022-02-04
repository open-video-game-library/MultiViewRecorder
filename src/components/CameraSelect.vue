<template>
    <select v-model="camera">
        <option :value="none" selected>{{ default_message }}</option>
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
        })().then(value => {
            console.log(value)
            this.default_message = 'Select a camera.'
        });
    },
    data() {
        return {
            camera_options: [],
            default_message: 'Loading...'
        }
    }
}
</script>

<style>
</style>
