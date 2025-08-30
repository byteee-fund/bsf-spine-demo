# bsf-spine UTS 插件演示项目

## 项目介绍

这是一个基于 **bsf-spine UTS 插件** 的演示项目，展示了如何在 uni-app 应用中集成和使用 Spine 动画引擎。该插件是一个专为 uni-app 开发的原生组件，支持在 Android 和 iOS 平台上播放 Spine 2D 动画。

## 🎯 主要特性

- ✅ **跨平台支持**: 支持 Android 和 iOS 平台
- ✅ **Spine 4.2 运行时**: 基于最新 Spine 4.2 动画引擎
- ✅ **动态加载**: 支持从网络下载和缓存 Spine 资源文件
- ✅ **动画控制**: 支持动画播放、暂停、切换等操作
- ✅ **实时调整**: 支持动画缩放、位置偏移等属性调整
- ✅ **事件监听**: 提供下载进度和加载完成事件监听
- ✅ **资源管理**: 自动缓存机制，提升加载性能

## 📦 安装与配置

### 1. 插件安装

在 uni-app 项目中安装 bsf-spine 插件：

```bash
# 通过 uni_modules 安装
# 将 uni_modules/bsf-spine 文件夹复制到您的项目中
```

### 2. 平台配置

#### Android 配置

插件已自动配置 Android 依赖，无需额外设置：
- 最低 SDK 版本: 24
- Spine Android 运行时: 4.2.10

#### iOS 配置

插件已自动配置 iOS 依赖，无需额外设置：
- 最低部署目标: iOS 13.0
- Spine iOS 运行时: 4.2
- Metal 渲染引擎支持

## 🚀 快速开始

### 基础用法

在页面中使用 bsf-spine 组件：

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
    ></bsf-spine>
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
      defaultAnimation: 'idle',
      loading: false,
      progress: 0,
      isLoaded: false
    }
  },
  methods: {
    onProgress(progress) {
      this.loading = true
      this.progress = progress
      console.log('下载进度:', progress + '%')
    },
    onLoad(loaded) {
      this.loading = false
      this.isLoaded = loaded
      console.log('Spine动画加载完成:', loaded)
    }
  }
}
</script>
```

## 📖 API 文档

### 组件属性

| 属性 | 类型 | 默认值 | 说明 |
|------|------|--------|---------|
| `src` | String | `""` | Spine 资源文件 URL（zip 格式） |
| `scale` | Number | `1` | 动画缩放比例（0.1 - 3.0） |
| `offsetX` | Number | `0` | 水平偏移量（-100 到 100） |
| `offsetY` | Number | `0` | 垂直偏移量（-100 到 100） |
| `defaultAnimation` | String | `"idle"` | 默认播放的动画名称 |

### 组件事件

| 事件名 | 参数 | 说明 |
|--------|------|---------|
| `@progress` | `progress: number` | 资源下载进度（0-100） |
| `@load` | `loaded: boolean` | 动画加载完成状态 |

## 📁 资源文件格式

### ZIP 包结构

Spine 资源文件必须打包成 ZIP 格式，包含以下必需文件：

```
spine-animation.zip
├── spine.atlas          # Atlas 纹理文件
├── spine.json           # Skeleton 骨骼数据文件  
├── spine.png            # 主纹理图片
└── [其他图片文件]        # 额外的纹理图片
```

### 文件要求

1. **必需文件**:
   - `spine.atlas` - 纹理图集描述文件
   - `spine.json` - 骨骼动画数据文件
   - 相应的 PNG 图片文件

2. **格式兼容性**:
   - 支持 Spine 4.2 版本导出的文件
   - 确保编辑器版本与运行时版本匹配

## 💡 使用示例

### 1. 动态切换动画

```vue
<template>
  <view>
    <bsf-spine
      ref="spineRef"
      :src="spineSrc"
      :defaultAnimation="currentAnimation"
    />
    
    <view class="controls">
      <button @click="playAnimation('idle')">待机</button>
      <button @click="playAnimation('walk')">行走</button>
      <button @click="playAnimation('attack')">攻击</button>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      spineSrc: 'https://example.com/character.zip',
      currentAnimation: 'idle'
    }
  },
  methods: {
    playAnimation(animationName) {
      this.currentAnimation = animationName
    }
  }
}
</script>
```

### 2. 实时属性调整

```vue
<template>
  <view>
    <bsf-spine
      :src="spineSrc"
      :scale="scale"
      :offsetX="offsetX"
      :offsetY="offsetY"
    />
    
    <view class="controls">
      <!-- 缩放控制 -->
      <view class="control-group">
        <text>缩放: {{ scale.toFixed(1) }}</text>
        <slider 
          :value="scale * 10" 
          @change="onScaleChange" 
          min="1" 
          max="30"
        />
      </view>
      
      <!-- X轴偏移控制 -->
      <view class="control-group">
        <text>X偏移: {{ offsetX }}</text>
        <slider 
          :value="offsetX + 100" 
          @change="onOffsetXChange" 
          min="0" 
          max="200"
        />
      </view>
      
      <!-- Y轴偏移控制 -->
      <view class="control-group">
        <text>Y偏移: {{ offsetY }}</text>
        <slider 
          :value="offsetY + 100" 
          @change="onOffsetYChange" 
          min="0" 
          max="200"
        />
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      spineSrc: 'https://example.com/spine-animation.zip',
      scale: 1.0,
      offsetX: 0,
      offsetY: 0
    }
  },
  methods: {
    onScaleChange(e) {
      this.scale = e.detail.value / 10
    },
    onOffsetXChange(e) {
      this.offsetX = e.detail.value - 100
    },
    onOffsetYChange(e) {
      this.offsetY = e.detail.value - 100
    }
  }
}
</script>
```

### 3. 加载状态处理

```vue
<template>
  <view>
    <bsf-spine
      :src="spineSrc"
      @progress="onProgress"
      @load="onLoad"
    />
    
    <!-- 加载进度显示 -->
    <view v-if="loading" class="loading-container">
      <view class="progress-bar">
        <view class="progress-fill" :style="{ width: progress + '%' }"></view>
      </view>
      <text>加载进度: {{ progress }}%</text>
    </view>
    
    <!-- 状态显示 -->
    <view class="status">
      <text :class="{ 'success': isLoaded, 'loading': loading }">
        {{ isLoaded ? '动画已加载' : loading ? '正在加载...' : '等待加载' }}
      </text>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      spineSrc: 'https://example.com/spine-animation.zip',
      loading: false,
      progress: 0,
      isLoaded: false
    }
  },
  methods: {
    onProgress(progress) {
      this.loading = true
      this.progress = progress
      
      if (progress < 0) {
        // 下载失败处理
        uni.showToast({
          title: '下载失败',
          icon: 'error'
        })
      }
    },
    onLoad(loaded) {
      this.loading = false
      this.isLoaded = loaded
      
      if (!loaded) {
        // 加载失败处理
        uni.showToast({
          title: '动画加载失败',
          icon: 'error'
        })
      }
    }
  }
}
</script>
```

## 🔧 技术实现

### Android 平台
- **运行时**: Spine Android 4.2.10
- **渲染**: 基于 `SpineView` 和 `SpineController`
- **加速**: 支持硬件加速渲染
- **最低版本**: Android API 24 (Android 7.0)

### iOS 平台
- **运行时**: Spine iOS 4.2
- **渲染**: 基于 `SpineView` 和 `SpineController`
- **引擎**: Metal 渲染引擎
- **最低版本**: iOS 13.0

## ⚠️ 注意事项

### 1. 平台支持
- 仅支持 App 平台（Android/iOS）
- 不支持 H5、小程序等其他平台

### 2. 版本兼容性
- 确保 Spine 编辑器版本与运行时版本匹配（推荐 4.2.x）
- 导出设置建议使用 JSON 格式而非二进制格式

### 3. 资源管理
- ZIP 文件会自动下载并缓存到应用本地目录
- 首次加载需要网络连接，后续使用缓存
- 建议使用 CDN 加速资源下载

### 4. 性能优化
- 大型动画建议合理设置纹理尺寸
- 避免同时加载过多动画实例
- iOS 平台建议主动释放不用的动画资源

### 5. 网络权限
- 确保应用具有网络访问权限
- 建议在 `manifest.json` 中配置网络权限

## 🗂️ 项目结构

```
bsf-spine-demo/
├── pages/
│   └── index/
│       └── index.uvue          # 主演示页面
├── uni_modules/
│   └── bsf-spine/              # bsf-spine 插件
│       ├── utssdk/
│       │   ├── app-android/    # Android 平台配置
│       │   └── app-ios/        # iOS 平台配置
│       ├── package.json        # 插件信息
│       ├── readme.md           # 插件说明
│       └── example.md          # 使用示例
├── manifest.json               # 应用配置
├── pages.json                  # 页面配置
└── README.md                   # 项目说明
```

## 🔗 相关链接

- [Spine 官方网站](https://esotericsoftware.com/)
- [Spine 4.2 文档](https://esotericsoftware.com/spine-user-guide)
- [uni-app 官方文档](https://uniapp.dcloud.net.cn/)
- [UTS 插件开发文档](https://uniapp.dcloud.net.cn/plugin/uts-plugin.html)

## 📝 开发日志

### v1.0.0
- ✅ 基础动画播放功能
- ✅ 支持 Android 和 iOS 平台
- ✅ 网络资源下载和缓存
- ✅ 动画属性实时调整
- ✅ 加载进度和状态监听

## 🤝 贡献指南

欢迎提交 Issue 和 Pull Request 来帮助改进项目。

## 📄 许可证

本项目基于 Spine 运行时开发，使用时请遵守 Spine 的相关许可协议。

---

**注意**: 本项目仅为演示用途，实际使用时请确保拥有合法的 Spine 许可证。