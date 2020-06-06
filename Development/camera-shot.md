# 浏览器调用摄像头拍照
前阵子项目做一个调摄像头拍照的需求，需求的实现和遇到的一些问题，在本文做一些总结和记录

## 需求实现
**HTML 容器**
```
<canvas id="camera-shot-canvas"></canvas>
<video id="camera-shot-video"></video>
<button onclick="shot">拍照</button>
```
**获取媒体设备**
```
const cameraDevices = []
navigator.mediaDevices.enumerateDevices().then(deviceInfos => {
  for (const deviceInfo of deviceInfos) {
    if (deviceInfo.kind === 'videoinput') {
      cameraDevices.push({
        deviceId: deviceInfo.deviceId,          // 摄像头的设备ID
        label: deviceInfo.label || 'camera',    // 摄像头的设备名称
      })
    }
  }
})
```
***调起摄像头 获取视频流 在video上播放***
```
// 兼容不同浏览器版本的getUserMedia
function getUserMedia(constrains, success, error) {
  if(navigator.mediaDevices.getUserMedia){
    //最新标准API
    navigator.mediaDevices.getUserMedia(constrains).then(success).catch(error);
  } else if (navigator.webkitGetUserMedia){
    //webkit内核浏览器
    navigator.webkitGetUserMedia(constrains).then(success).catch(error);
  } else if (navigator.mozGetUserMedia){
    //Firefox浏览器
    navagator.mozGetUserMedia(constrains).then(success).catch(error);
  } else if (navigator.getUserMedia){
    //旧版API
    navigator.getUserMedia(constrains).then(success).catch(error);
  }
}

const videoDom = document.querySelector('#camera-shot-video')
getUserMedia({
  // video可直接传true，deviceId没传参数则使用默认的
  video: {
    width: 500, 
    height: 500,
    deviceId: cameraDevices.length && cameraDevices[0].deviceId ? { exact: cameraDevices[0].deviceId } : undefined
  },
  audio: false,
}, stream => {
  // 播放视频流(兼容旧版video src)
  if ("srcObject" in videoDom) {
    videoDom.srcObject = stream
  } else {
    videoDom.src = window.URL.createObjectURL(stream)
  }
  videoDom.onloadedmetadata = function(e) {
    videoDom.play()
  }
}, err => {
  console.log(err);
  that.$message.error('摄像头开启失败！')
})

```
**截取video视频画面 绘制到canvas上**
```
const canvasDom = document.querySelector('#camera-shot-canvas')
function shot() {
  canvasDom.getContext('2d').drawImage(
    videoDom,
    0,
    0,
    500,
    500
  )
}
```

核心就是 `navigator.mediaDevices.getUserMedia` API调用摄像头获取视频流，在video DOM上播放，再截取video视频画面 绘制到canvas上。  
其实这个功能实现也是很简单的，记录的主要在于后面的坑。

## 无法调用摄像头权限问题

### Http协议
现在的Chrome和其他大部分的现代浏览器，本地localhost开发、https协议的都可以正常调起摄像头，http想要调起摄像头则需要额外的浏览器设置。
- 打开链接 chrome://flags/#unsafely-treat-insecure-origin-as-secure
- #unsafely-treat-insecure-origin-as-secure 输入框中填写白名单 协议域名
- 将该 flag 切换成 enable 状态，Relaunch 重启浏览器即可

### 系统隐私权限
在个别的两三台电脑上 调用 `navigator.mediaDevices.getUserMedia` 浏览器上看到的是已获取摄像头权限，也拿到了 `deviceId` ，但始终报错 `Domexception: could not start video source`。 刚开始一直摸不着头脑，以为是播放视频流的问题。后面查到了问题，这几台电脑装都是Windows 10 神州网信政府版被系统禁掉了很多权限。

- win+R 输入gpedit.msc 并确定

- 找到： 计算机配置->管理模板->Windows组件->应用隐私->允许Windows应用访问相机

- 设置为 “已启用”

- 在下面的选项中，将“所有应用的默认设置”改成“由用户控制”或“强制允许”

- 重启计算机，就可以正常的调用摄像头了