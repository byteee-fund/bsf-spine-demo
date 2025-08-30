# Jeek Spine 组件

这是一个基于 Spine 动画引擎的 uni-app 原生组件，支持 Android 和 iOS 平台。

## 功能特性

- ✅ 支持 Spine 4.2 运行时
- ✅ 支持 Android 和 iOS 平台
- ✅ 支持动画播放、暂停、切换
- ✅ 支持缩放、位置调整
- ✅ 支持动画事件监听
- ✅ 支持资源文件下载和缓存

## 安装配置

### 1. 依赖配置

#### Android 配置
在 `uni_modules/bsf-spine/utssdk/app-android/config.json` 中已配置：
```json
{
  "minSdkVersion": 24,
  "dependencies": [
    "com.esotericsoftware.spine:spine-android:4.2.10"
  ]
}
```

#### iOS 配置
在 `uni_modules/bsf-spine/utssdk/app-ios/config.json` 中已配置：
```json
{
  "deploymentTarget": "13.0",
  "dependencies-pods": [
    {
      "name": "Spine",
      "podspec": "https://raw.githubusercontent.com/EsotericSoftware/spine-runtimes/4.2/Spine.podspec"
    },
    {
      "name": "SpineCppLite",
      "podspec": "https://raw.githubusercontent.com/EsotericSoftware/spine-runtimes/4.2/SpineCppLite.podspec"
    },
    {
      "name": "SpineShadersStructs",
      "podspec": "https://raw.githubusercontent.com/EsotericSoftware/spine-runtimes/4.2/SpineShadersStructs.podspec"
    }
  ]
}
```

### 2. 使用组件

```vue
<template>
  <view class="container">
    <bsf-spine
      :src="spineSrc"
      :scale="spineScale"
      :offsetX="spineOffsetX"
      :offsetY="spineOffsetY"
      :defaultAnimation="defaultAnimation"
      @progress="onProgress"
      @load="onLoad"
    />
  </view>
</template>

<script>
export default {
  data() {
    return {
      spineSrc: 'https://example.com/spine-animation.zip',
      spineScale: 1.0,
      spineOffsetX: 0,
      spineOffsetY: 0,
      defaultAnimation: 'idle'
    }
  },
  methods: {
    onProgress(progress) {
      console.log('下载进度:', progress + '%')
    },
    onLoad(loaded) {
      console.log('Spine动画加载完成:', loaded)
    }
  }
}
</script>
```

## 组件属性

| 属性 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| src | String | "" | Spine 资源文件 URL（zip 格式） |
| scale | Number | 1 | 动画缩放比例 |
| offsetX | Number | 0 | 水平偏移量（-100 到 100） |
| offsetY | Number | 0 | 垂直偏移量（-100 到 100） |
| defaultAnimation | String | "idle" | 默认播放的动画名称 |

## 组件事件

| 事件 | 参数 | 说明 |
|------|------|------|
| progress | progress: number | 下载进度事件 |
| load | loaded: boolean | 加载完成事件 |

## 资源文件格式

组件支持从远程 URL 下载 Spine 资源文件，文件格式要求：

1. **文件格式**: ZIP 压缩包
2. **必需文件**:
   - `spine.atlas` - Atlas 文件
   - `spine.json` - Skeleton 文件
   - 相关的图片文件

## 示例项目

项目包含完整的示例，展示了：
- 基本动画播放
- 动画属性调整
- 下载进度显示
- 多动画切换

## 技术实现

### Android 平台
- 使用 Spine Android 运行时 4.2.10
- 基于 `SpineView` 和 `SpineController`
- 支持硬件加速渲染

### iOS 平台
- 使用 Spine iOS 运行时 4.2
- 基于 `SpineView` 和 `SpineController`
- 使用 Metal 渲染引擎

## 注意事项

1. **版本兼容性**: 确保 Spine 编辑器版本与运行时版本匹配
2. **资源文件**: 确保资源文件格式正确，包含所有必需文件
3. **内存管理**: iOS 平台需要主动调用 `destroy()` 方法释放资源
4. **网络权限**: 确保应用有网络访问权限用于下载资源

## 许可证

本项目基于 Spine 运行时，请遵守 Spine 的许可协议。