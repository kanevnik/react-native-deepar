# react-native-deepar <!-- omit in toc -->

[![npm version](https://img.shields.io/npm/v/react-native-deepar.svg)](https://npmjs.com/package/react-native-deepar)
[![CircleCI](https://circleci.com/gh/ridvanaltun/react-native-deepar/tree/master.svg?style=shield)](https://circleci.com/gh/ridvanaltun/react-native-deepar/tree/master)
[![Commitizen friendly](https://img.shields.io/badge/commitizen-friendly-brightgreen.svg)](http://commitizen.github.io/cz-cli/)
[![license](https://img.shields.io/npm/l/react-native-deepar.svg)](https://github.com/ridvanaltun/react-native-deepar/blob/master/LICENSE)

> Snapchat-like filters, AR lenses, and real-time facial animations.

> React-Native wrapper for [DeepAR](https://www.deepar.ai/).

## Table of Contents <!-- omit in toc -->

- [What is DeepAR?](#what-is-deepar)
- [Getting Started](#getting-started)
- [Compatibility](#compatibility)
- [Installing AR Models](#installing-ar-models)
- [Using AR Models over Internet](#using-ar-models-over-internet)
- [Usage](#usage)
- [API](#api)
  - [Props](#props)
  - [Events](#events)
  - [Methods](#methods)
    - [General](#general)
    - [Camera Control](#camera-control)
    - [Video Recording](#video-recording)
    - [Change Parameters](#change-parameters)
    - [Core](#core)
- [Background Segmentation](#background-segmentation)
- [Example App](#example-app)
- [Contributing](#contributing)
- [License](#license)

## What is DeepAR?

| Preview                                                          |
| ---------------------------------------------------------------- |
| <img src="./docs/preview.gif" alt="Preview Image" width="600" /> |
| This GIF taken from DeepAR offical site.                         |

DeepAR is an infrastructure where you can make AR applications in ease. DeepAR is not free, but you can create applications that can be used by up to 10 people for testing purposes for free.

- In order to use DeepAR, you need to generate an API key. You can generate the API key from the [Developer Panel](https://developer.deepar.ai/).
- With the [Asset Store](https://www.store.deepar.ai/), you can buy ready to use AR content. If you're looking for free filters, here is some: [Free Filter Pack](https://help.deepar.ai/en/articles/3580432-free-filter-pack)
- With the [DeepAR Studio](https://www.deepar.ai/creator-studio), you can create, edit and fine tune your own AR content. To learn DeepAR Studio, visit [DeepAR Help Center](https://help.deepar.ai/en/).

You can visit [DeepAR's offical site](https://www.deepar.ai/) to learn more.

## Getting Started

:warning: This library under development, if you found a bug, please open an issue from [here](https://github.com/ridvanaltun/react-native-deepar/issues/new).

:warning: It only works on physical devices, not will work with simulator.

```sh
npm install react-native-deepar
```

**Note:** Don't forget install Pods for iOS and rebuild your app.

## Compatibility

| DeepAR SDK | lib version                      | Required React Native Version |
| ---------- | -------------------------------- | ----------------------------- |
| 3.4.2      | `react-native-deepar` `>= 0.1.0` | unknown                       |

## Installing AR Models

1. Put your AR models into a desired destination
2. Create `react-native.config.js` like below:

```js
module.exports = {
  assets: ['./assets/effects'], // <-- example destination
};
```

3. Add a NPM script like below:

```diff
{
  "scripts": {
+   "asset": "./node_modules/.bin/react-native-deepar-link"
  }
}
```

4. Run the script to link your AR models

```sh
npm run asset
```

**Note:** If you remove an AR model, you can run the same command for unlinking removed asset.

## Using AR Models over Internet

1. Install [rn-fetch-blob](https://github.com/joltup/rn-fetch-blob) to your project
2. You can use AR models over internet like below:

```tsx
import RNFetchBlob from 'rn-fetch-blob';

RNFetchBlob.config({
  fileCache: true,
})
  .fetch('GET', 'http://betacoins.magix.net/public/deepar-filters/8bitHearts')
  .then((res) => {
    deepARRef?.current?.switchEffectWithPath({
      path: res.path(),
      slot: 'effect',
    });
  });
```

## Usage

Make registration to DeepAR and get an API key from [Developer Panel](https://developer.deepar.ai/).

```tsx
import React, {useRef} from 'react';
import DeepAR, {IDeepARHandle} from 'react-native-deepar';

const App = () => {
  const deepARRef = useRef<IDeepARHandle>(null);

  return (
    <DeepAR
      ref={deepARRef}
      apiKey="your-api-key"
      style={{flex: 1}}
      onInitialized={() => {
        // ..
      }}
    />
  );
};
```

## API

The `<DeepAR>` component can take a number of inputs to customize it as needed. They are outlined below:

### Props

| Prop   | Type   | Default | Required | Description                                                                                          |
| ------ | ------ | ------- | -------- | ---------------------------------------------------------------------------------------------------- |
| apiKey | String | null    | true     | Make registration to DeepAR and get an API key from [Developer Panel](https://developer.deepar.ai/). |

### Events

These are various events that you can hook into and fire functions on in the component:

| Callback                 | Description                                                                                                                     |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------- |
| onInitialized            | Called when the DeepAR is initialized. DeepAR methods should not be called before the initialization is completed.              |
| onEffectSwitched         | Called when an effect has been switched.                                                                                        |
| onScreenshotTaken        | Called when the screen capture is finished.                                                                                     |
| onVideoRecordingPrepared | Called when the video recording is prepared.                                                                                    |
| onVideoRecordingStarted  | The start of the video recording process is not synchronous, so this method will be called when the video recording is started. |
| onVideoRecordingFinished | Called when the video recording is finished.                                                                                    |
| onCameraSwitched         | Fired when camera switched.                                                                                                     |
| onFaceVisibilityChanged  | Called when the user's face becomes visible or invisible.                                                                       |
| onImageVisibilityChanged | Called when a natural image is being tracked and the visibility has changed.                                                    |
| onError                  | Called when an error occur, like the model path not found or the effect file failed to load.                                    |

### Methods

These are the various methods.

#### General

| Method               | Description                                                                                                                                                                                                                                                                  |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| switchEffect         | The method used to switch any effect in the scene. Effects are places in slots. Every slot is identified by its unique name and can hold one effect at any given moment. Every subsequent call to this method removes the effect that was previously displayed in this slot. |
| switchEffectWithPath | Same as `switchEffect` but with path.                                                                                                                                                                                                                                        |
| fireTrigger          | This method allows the user to fire a custom animation trigger for model animations from code. To fire a custom trigger, the trigger string must match the custom trigger set in the Studio when creating the effect.                                                        |
| takeScreenshot       | Captures a screenshot of the current screen. When a screenshot is done `onScreenshotTaken` will be called with a resulting screenshot.                                                                                                                                       |

#### Camera Control

| Method       | Description                          |
| ------------ | ------------------------------------ |
| switchCamera | Switches the camera, back and front. |
| setFlashOn   | Toggle flash.                        |

#### Video Recording

| Method          | Description                                                                                                                                                                 |
| --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| startRecording  | Starts video recording of the camera preview.                                                                                                                               |
| pauseRecording  | Pauses video recording.                                                                                                                                                     |
| resumeRecording | Resumes video recording after it has been paused with `pauseRecording`.                                                                                                     |
| finishRecording | Stops video recording and starts the process of saving the recorded video to the file system. When the file is saved, the method `onVideoRecordingFinished` will be called. |
| setAudioMute    | Mutes/unmutes the audio while video recording.                                                                                                                              |

#### Change Parameters

For more details about changeParameter API read our article [here](https://help.deepar.ai/en/articles/3732006-changing-filter-parameters-from-code).

| Method                 | Description                                                                                                                                                                                                                            |
| ---------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| changeParameterFloat   | This method allows the user to change the value of blendshape parameters during runtime.                                                                                                                                               |
| changeParameterVec4    | This method is used to change the certain color of a Game Object at runtime.                                                                                                                                                           |
| changeParameterVec3    | This method is used to change the transform of a Game Object at runtime, so here you can change the object position, rotation or scale.                                                                                                |
| changeParameterBool    | Let say you want to put a button in your app that enables or disables Game Object at runtime. (let's say you want your filter character to put their glasses on or take them off) This function helps you to enable/disable the value. |
| changeParameterString  | Change a string parameter on a game object. The most common use for this override is to change blend mode and culling mode properties of a game object. **Note:** Only available in iOS                                                |
| changeParameterTexture | This method allows the user to load an image and set it as a texture during runtime. This can be useful if you want to leverage our background segmentation feature, and change the background in your filter.                         |

#### Core

| Method                      | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| --------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| pause                       | Pauses the rendering. This method will not release any resources and should be used only for temporary pause (e.g. user goes to the next screen).                                                                                                                                                                                                                                                                                                                                                                                                                |
| resume                      | Resumes the rendering if it was previously paused, otherwise doesn't do anything.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| setLiveMode                 | This is an optimization method and it allows the user to indicate the DeepAR in which mode it should operate. If called with true value, DeepAR will expect a continuous flow of new frames and it will optimize its inner processes for such workload. An example of this is the typical use case of processing the frames from the camera stream. If called with false it will optimize for preserving resources and memory by pausing the rendering after each processed frame. A typical use case for this is when the user needs to process just one image. |
| setFaceDetectionSensitivity | This method allows the user to change face detection sensitivity. The sensitivity parameter can range from 0 to 3, where 0 is the fastest but might not recognize smaller (further away) faces, and 3 is the slowest but will find smaller faces. By default, this parameter is set to 1.                                                                                                                                                                                                                                                                        |
| showStats                   | Display debugging stats on screen.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |

## Background Segmentation

DeepAR has [Background Segmentation](https://www.deepar.ai/background-removal) feature, with this feature you can change your background in real-time.

![Background Segmentation](./docs/background-segmentation.png)

There is a filter called `Background` from [Free Filter Pack](https://help.deepar.ai/en/articles/3580432-free-filter-pack) and you can use this filter.

**How change background image?**

Switch the `Background` effect and apply new background image like below:

```js
import {TextureSourceTypes} from 'react-native-deepar';
import RNFetchBlob from 'rn-fetch-blob';

RNFetchBlob.config({})
  .fetch('GET', 'https://random.imagecdn.app/450/800')
  .then((res) => {
    deepARRef?.current?.changeParameterTexture({
      gameObject: 'Background',
      component: 'MeshRenderer',
      parameter: 's_texColor',
      type: TextureSourceTypes.BASE64,
      value: res.base64(),
    });
  });
```

## Example App

```sh
# clone the project
git clone https://github.com/ridvanaltun/react-native-deepar.git

# go into the project
cd react-native-deepar

# make project ready
npm run bootstrap

# go into the example
cd example

# copy environment file and set your api keys (bundle id is com.example.reactnativedeepar)
cp .env.example .env

# run for android
npm run android

# or

# run for ios
npm run ios
```

## Contributing

See the [contributing guide](CONTRIBUTING.md) to learn how to contribute to the repository and the development workflow.

## License

This project is licensed under the [MIT License](https://opensource.org/licenses/MIT) - see the [`LICENSE`](LICENSE) file for details.
