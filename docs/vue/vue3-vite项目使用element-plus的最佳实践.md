<!--
 * @Author: dingyuwen
 * @Date: 2022-08-11 09:39:22
 * @LastEditTime: 2022-08-12 11:19:19
 * @LastEditors: dingyuwen
 * @Description: 
-->
# vue3-vite项目使用element-plus的最佳实践

最近在从零搭建一个vue3+vite+pinia+element-plus的开发环境<br>
在项目中引入element-plus时发现样式失效的问题，以及icon图标使用的问题，特此记录一下解决方案。


**先挖坑！！！**<br>
**晚点来填！！！**

## element-plus组件使用

---

### 组件全部导入
### 组件按需导入
#### 手动按需导入
#### 自动按需导入(带样式)

## Icon图标的使用

---

### 安装方式
#### 注册所有图标
#### 按需导入图标

### 使用方式

#### 直接使用 SVG 图标

```
<template>
  <Expand />
  <Fold />
</template>

<script setup>
import { Expand, Fold } from '@element-plus/icons-vue';
</script>
```
#### 结合 el-icon 使用
```
<template>
  <el-icon color="#000" size="22">
    <Expand />
  </el-icon>

  <el-icon style="color: #000; font-size: 22px;">
    <Fold />
  </el-icon>
</template>

<script setup>
import { Expand, Fold } from '@element-plus/icons-vue';
</script>
```

#### 使用iconify图标库


#### 使用自定义 SVG Icon


## 最佳实践
```js
// vite.config

import { resolve } from 'path'
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import AutoImport from 'unplugin-auto-import/vite'
import Components from 'unplugin-vue-components/vite'
import { ElementPlusResolver } from 'unplugin-vue-components/resolvers'
import Icons from 'unplugin-icons/vite'
import IconsResolver from 'unplugin-icons/resolver'

const pathSrc = resolve(__dirname, 'src')

export default defineConfig({
  plugins: [
    vue(),
    // 自动导入vue3的hooks
    AutoImport({
      // 自动导入 hooks
      imports: ['vue', 'vue-router','@vueuse/core'],
      dirs: [
        resolve(pathSrc, 'composables'),
        resolve(pathSrc, 'hooks'),
      ],
      resolvers: [
        // 自动导入 ElMessage, ElMessageBox... (带样式)
        ElementPlusResolver(),
        // 自动导入图标组件
        IconsResolver({
          prefix: 'Icon',
        }),
      ],
      vueTemplate: true,
      dts: resolve(pathSrc, 'auto-imports.d.ts'),
    }),
    // 自动导入 Element Plus 组件
    Components({
      extensions: ['vue', 'md'],
      include: [/\.vue$/, /\.vue\?vue/, /\.md$/],
      resolvers: [
        ElementPlusResolver({
          importStyle: 'sass',
        }),
        // 自动注册图标组件
        IconsResolver({
          enabledCollections: ['ep', 'carbon'],
        }),
      ],
      dts: resolve(pathSrc, 'components.d.ts'),
    }),
    Icons({
      autoInstall: true,
    }),
  ]
})
```

```HTML
<template>
    <div>
        <el-space>
            <div i-ep-expand />
            <i-ep-add-location />
            <i-ep-aim />
            <i-carbon-cyclist />
            <div i-ep-check />
            <div i-ep-fold />
        </el-space>
    </div>
</template>
```
