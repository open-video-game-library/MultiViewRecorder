# Multi View Recorder

Multi View Recorder is a web application that records multiple webcam videos.

Application: https://open-video-game-library.github.io/multi-view-recorder/

https://user-images.githubusercontent.com/52689532/208599303-5f6a3006-fb9d-477f-8f25-158b96b4683b.mp4


## Contents

Up to four videos can be recorded and downloaded simultaneously.

Select the camera device from the pull-downs in the four corners of the canvas.

In addition to camera video, PC display and audio waveforms can also be displayed.

Press the "Start recording" button at the bottom of the canvas to start recording.

Pressing the same button again will stop recording and start downloading the video.


## Features

### Parameters

You can change parameters related to recording from settings(the gear icon in the upper right corner).

- Size: Size of the screen to record.

   640\*360 / 1280\*720 / 1920\*1080

- Frame rate (canvas): How often the canvas is updated (not the frame rate of the camera input)

   15 / 30 / 60

- Mimetype: Output video format

   "video/webm" is recommended.

### Research Applications

1. Collection of speech data using the retrospective think-aloud method.

  Participants reflect on their own behavior using videos during the task recorded by the Multi View Recorder.


## Requirement

vue.js: ^3.0.0


## Installation

Data in this repository can be cloned to the local environment by entering the following command.

```
git clone git@github.com:open-video-game-library/multi-view-recorder.git
```


## Licence

This content is licensed under the MIT License.


## Citiation

HCI研究における評価実験用ビデオゲームの要件探究とオープンビデオゲームライブラリを用いたケーススタディ

Exploring Requirements for Evaluation Experimental Video Games in HCI Research and Case Studies Using Open Video Game Library.

[Click here for the paper page.](http://id.nii.ac.jp/1001/00214482/)

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


## Contact

If you have any comments, requests or questions, please contact us [here](https://open-video-game-library.github.io/info/contact/).
