![Example](https://bs.vaban.ru/logo.jpg)

# Vue Bottom Sheet Vue 2

![Size](https://img.shields.io/bundlephobia/minzip/@webzlodimir/vue-bottom-sheet-vue2)
![Downloads](https://img.shields.io/npm/dt/@webzlodimir/vue-bottom-sheet-vue2)
![Version](https://img.shields.io/npm/v/@webzlodimir/vue-bottom-sheet-vue2)

一个漂亮干净且触摸友好的底板组件，基于 [Vue.js 2](https://vuejs.org/)
和 [hammer.js](https://hammerjs.github.io/)

- [Vue 3 Version](https://github.com/vaban-ru/vue-bottom-sheet)
- :clapper: [Demo](https://bs2.vaban.ru/)
- [英文文档](https://github.com/vaban-ru/vue-bottom-sheet-vue2)

## 安装

### NPM

```
npm install @webzlodimir/vue-bottom-sheet-vue2
```

### Yarn

```
yarn add @webzlodimir/vue-bottom-sheet-vue2
```

## 使用

```vue

<template>
    <vue-bottom-sheet-vue2 ref="myBottomSheet">
        <h1>Lorem Ipsum</h1>
        <h2>What is Lorem Ipsum?</h2>
        <p>
            <strong>Lorem Ipsum</strong> is simply dummy text
        </p>
    </vue-bottom-sheet-vue2>
</template>

<script>
import VueBottomSheetVue2 from "@webzlodimir/vue-bottom-sheet-vue2";

export default {
    components: {
        VueBottomSheetVue2
    },
    methods: {
        open() {
            this.$refs.myBottomSheet.open();
        },
        close() {
            this.$refs.myBottomSheet.close();
        }
    }
}
</script>
```

## Props

| Prop                          | Type    | Description                       | Example                        | Defaults  |
|:------------------------------|:--------|:----------------------------------|:-------------------------------|:----------|
| max-width                     | Number  | 设置组件的最大宽度，单位px                    | `:max-width="640"`             | 640       |
| max-height                    | Number  | 设置窗口的最大高度，如果未设置，则采用内容的高度          | `:max-height="90"`             | -         |
| can-swipe                     | Boolean | 启用或禁用滑动关闭                         | `:can-swipe="false"`           | true      |
| overlay                       | Boolean | 是否启用遮罩层                           | `:overlay="false"`             | true      |
| overlay-color                 | String  | 遮罩层颜色                             | `overlay-color="#0000004D"`    | #0000004D |
| overlay-click-close           | Boolean | 点击遮罩层是否可关闭卡片                      | `:overlay-click-close="false"` | true      |
| close-height-percent `v0.0.7` | Number  | 设置关闭状态时占比全开时的高度百分比，取值0-100，100为全关 | `:max-height="50"`             | 100       |
| drag-color `v0.0.8`           | String  | 拖拽栏图标颜色                           | `drag-color="#ffc107"`         | #333333   |
| custom-class `v0.0.8`         | String  | 组件自定义类名                           | `custom-class="custom-style"`  | ''        |
| init-sheet-height `v0.0.8`    | Number  | 设置窗口的初始高度，如果未设置，则采用内容高度           | `:init-sheet-height="300"`     | -         |
| z-index `v0.0.8`              | Number  | 设置窗口的z-index                      | `:z-index="1001"`              | 99999     |
| transition-duration           | Number  | 过渡动画持续时间                          | `:transition-duration="0.5"`   | 0.5       |

### custom-class说明

```vue

<template>
    <vue-bottom-sheet-vue2
        custom-class="bg-color"
    />
</template>

<style lang="scss" scoped>
::v-deep .bg-color {
    background-color: #ffc107;
}
</style>
```

or:

```vue

<template>
    <vue-bottom-sheet-vue2
        custom-class="bg-color"
    />
</template>

<style lang="scss">
.bg-color {
    background-color: #ffc107;
}
</style>
```

## Events

| Event              | Description              | Example                    |
|:-------------------|:-------------------------|:---------------------------|
| opened             | 卡片组件打开时触发                | @opened=""                 |
| closed             | 卡片组件关闭时触发                | @closed=""                 |
| dragging-up        | 卡片组件向上拖动时触发              | @dragging-up=""            |
| dragging-down      | 卡片组件向下拖动时触发              | @dragging-down=""          |

## Slots

You can use this named slots:

| Slot           | Description | 
|:---------------|:------------|
| drag `v1.2.0`  | 拖拽栏图标       | 
| header         | 眉头内容        | 
| default        | 主体内容        |
| footer         | 眉脚内容        |

```vue

<template>
    <vue-bottom-sheet-vue2 ref="myBottomSheet">
        <template #drag>
            <div style="width: 40px;height: 4px;background: #31d2f2;border-radius: 8px;margin: 0 auto;"></div>
        </template>
        <template #header>
            <h1>Lorem Ipsum</h1>
        </template>
        <template #default>
            <h2>What is Lorem Ipsum?</h2>
        </template>
        <template #footer>
            <p>
                <strong>Lorem Ipsum</strong> is simply dummy text
            </p>
        </template>
    </vue-bottom-sheet-vue2>
</template>
```
