<template>
    <div class="background-shadow" @click.self="closeInformation">
        <div class="popup">
            <header class="wrapper">
                <h2>How to use</h2>
                <p id="btn-icon-times" class="btn-icon" @click="closeInformation">
                    <fa icon="times" />
                </p>
            </header>
            <main class="wrapper">
                <section>
                    <p>ChromeとFirefoxで動作します。<span class="red">Chrome推奨です。</span></p>
                </section>
                <section>
                    <div class="flex-container">
                        <h3>① 録画設定</h3>
                        <p class="btn-icon" @click="openSettings"><fa icon="cog" /></p>
                    </div>
                    <p><u>Size</u>: 録画する画面のサイズ。3段階から選択できます。</p>
                    <p><u>Frame rate (canvas)</u>: キャンバスの更新頻度です。カメラのインプットのフレームレートではありません。</p>
                    <p><u>Mimetype</u>: 出力する動画の形式です。<span class="red">「video/webm」推奨です。</span></p>
                </section>
                <section>
                    <h3>② カメラとマイクを選択</h3>
                    <p>録画したいカメラを選択してください（最大4カメラ）。</p>
                    <p>音声の波形も表示させることができます。その場合は先にマイクを選択してから「Audio waveform」を選択してください。</p>
                    <p>別PCの画面を表示させる場合は、別途キャプチャボードが必要です。</p>
                    <img :src="imgPath_selectCamera" alt="Tutorial - Selecting camera" />
                </section>
                <section>
                    <h3>③ 録画開始＆終了</h3>
                    <p>「Start recording」を押すと録画が開始します。</p>
                    <p>「Stop recording」を押すと録画が終了します。</p>
                    <p>録画を終了すると自動的に動画がダウンロードされます。</p>
                </section>
            </main>
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
        },
        openSettings() {
            this.$emit('close-information')
            this.$emit('open-settings')
        }
    },
    data() {
        return {
            imgPath_selectCamera: require('@/assets/camera-select.gif')
        }
    }
}
</script>

<style lang="scss" scoped>
header {
    display: flex;
    justify-content: space-between;
}
.wrapper {
    margin: 0 auto;
    padding: 0 24px;
}
main {
    section {
        margin-bottom: 24px;
    }
    h3 {
        text-align: left;
        margin-bottom: 8px;
    }
    p {
        padding-left: 24px;
        margin: 12px 0 12px;
        text-align: left;
    }
    img {
        width: 80%;
    }
}
.flex-container {
    display: flex;
    align-items: top;
    height: auto;
    * { margin-top: 0; }
    .btn-icon {
        padding-left: 6px;
        font-size: 18px;
        margin-top: -1.5px;
    }
}
</style>