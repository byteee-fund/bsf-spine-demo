# Jeek Spine 组件使用示例

## 基本用法

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

## 动态切换动画

```vue
<template>
  <view class="container">
    <bsf-spine
      ref="spineRef"
      :src="spineSrc"
      :defaultAnimation="currentAnimation"
    />
    
    <view class="controls">
      <button @click="playIdle">播放待机动画</button>
      <button @click="playWalk">播放行走动画</button>
      <button @click="playAttack">播放攻击动画</button>
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
    playIdle() {
      this.currentAnimation = 'idle'
    },
    playWalk() {
      this.currentAnimation = 'walk'
    },
    playAttack() {
      this.currentAnimation = 'attack'
    }
  }
}
</script>
```

## 调整动画属性

```vue
<template>
  <view class="container">
    <bsf-spine
      :src="spineSrc"
      :scale="scale"
      :offsetX="offsetX"
      :offsetY="offsetY"
    />
    
    <view class="controls">
      <slider :value="scale * 10" @change="onScaleChange" min="1" max="30" />
      <text>缩放: {{ scale.toFixed(1) }}</text>
      
      <slider :value="offsetX + 100" @change="onOffsetXChange" min="0" max="200" />
      <text>X偏移: {{ offsetX }}</text>
      
      <slider :value="offsetY + 100" @change="onOffsetYChange" min="0" max="200" />
      <text>Y偏移: {{ offsetY }}</text>
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

## 资源文件格式

Spine 资源文件需要打包成 ZIP 格式，包含以下文件：

```
spine-animation.zip
├── spine.atlas          # Atlas 文件
├── spine.json           # Skeleton 文件
├── spine.png            # 图片文件
└── ...                  # 其他相关图片文件
```

## 注意事项

1. **文件格式**: 只支持 ZIP 格式的资源包
2. **必需文件**: 必须包含 `spine.atlas` 和 `spine.json` 文件
3. **网络权限**: 确保应用有网络访问权限
4. **平台支持**: 仅支持 Android 和 iOS 平台
5. **版本兼容**: 确保 Spine 编辑器版本与运行时版本匹配

## 错误处理

```vue
<script>
export default {
  methods: {
    onLoad(loaded) {
      if (loaded) {
        console.log('动画加载成功')
      } else {
        console.error('动画加载失败')
        // 显示错误提示
        uni.showToast({
          title: '动画加载失败',
          icon: 'error'
        })
      }
    },
    onProgress(progress) {
      if (progress === 100) {
        console.log('下载完成')
      } else if (progress < 0) {
        console.error('下载失败')
        // 处理下载失败
      }
    }
  }
}
</script>
``` 