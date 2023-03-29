### 音频播放器

### 描述
微信小程序、H5 音频播放器插件。

### 使用
采用```easycom```模式加载

使用```<gurudin-audio></gurudin-audio>```
|  属性    | 类型    | 必填 | 默认值 | 描述 |
|  :----  | :----  | :--- | :---  | :--- |
| poster  | String | 否 | | 封面 |
| author  | String | 否 | | 演唱者 |
| name | String | 否 | | 音频名称 |
| src | String | 是 | | 音频链接 |
| theme | String | 否 | light | 主题 light、dark |
| loop | Boolean | 否 | false | 是否循环播放 |
| obeyMuteSwitch | Boolean | 否 | false | 是否遵循系统静音开关 |
| call | Function | 否 | | 点击播放按钮回调方法 (传回当前播放src) |

### 示例
```
<gurudin-audio
        src="https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-hello-uniapp/2cc220e0-c27a-11ea-9dfb-6da8e309e0d8.mp3"
        poster="https://bjetxgzv.cdn.bspapp.com/VKCEYUGU-uni-app-doc/7fbf26a0-4f4a-11eb-b680-7980c8a877b8.png"
        name="致爱丽丝"
        author="路德维希·范·贝多芬"
        :call="playCall"
        theme="dark"
        ref="audioPlayers"
    ></gurudin-audio>
```