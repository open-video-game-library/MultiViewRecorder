# Multi View Recorder

[English version README](https://github.com/open-video-game-library/multi-view-recorder/blob/master/README.md)

Multi View Recorderは、複数のカメラ映像を録画できるWebアプリケーションです。

こちらから利用できます: https://open-video-game-library.github.io/multi-view-recorder/

https://user-images.githubusercontent.com/52689532/208599303-5f6a3006-fb9d-477f-8f25-158b96b4683b.mp4


## コンテンツ

最大4つの映像を同時に録画、ダウンロードできます。

キャンバスの4隅にあるプルダウンから、カメラデバイスを選択します。

カメラ映像だけでなく、PCのディスプレイや音声波形も表示できます。

キャンバスの下にある「Start recorsing」ボタンを押すと、録画を開始します。

もう一度同じボタンを押すことで録画が停止し、動画のダウンロードが始まります。


## 機能

### パラメータ

右上にある歯車アイコンから、録画に関するパラメータを変更できます。

- Size: 録画する画面のサイズ
   640*360 / 1280*720 / 1920*1080

- Frame rate (canvas): キャンバスの更新頻度(カメラのインプットのフレームレートではありません)
   15 / 30 / 60

- Mimetype: 出力する動画の形式
   「video/webm」推奨です。

### 研究利用例

1. レトロスペクティブシンクアラウド法による発話データの収集

Multi View Recorderで録画したタスク中の動画を用いて、実験参加者が自身の行動を振り返る。


## 環境

vue.js: ^3.0.0


## インストール方法

本リポジトリのデータは下記のコマンドを入力することでローカル環境にクローンできます。

```
git clone git@github.com:open-video-game-library/multi-view-recorder.git
```


## ライセンス

本コンテンツは、MITライセンスのもとで利用が許可されています。


## 引用

[HCI研究における評価実験用ビデオゲームの要件探究とオープンビデオゲームライブラリを用いたケーススタディ](http://id.nii.ac.jp/1001/00214482/)

```
@inproceedings{weko_214590_1,
   author	 = "拓也,岡 and 拓也,川島 and 大智,林 and 恵太,渡邊",
   title	 = "HCI研究における評価実験用ビデオゲームの要件探究とオープンビデオゲームライブラリを用いたケーススタディ",
   booktitle	 = "研究報告ヒューマンコンピュータインタラクション（HCI）",
   year 	 = "2022",
   volume	 = "2022-HCI-196",
   number	 = "12",
   pages	 = "1--8",
   month	 = "jan"
}
```


## お問い合せ

意見や要望、質問などがありましたら、[こちら](https://open-video-game-library.github.io/info/contact/)からお問い合わせ下さい。
