![Example](https://bs.vaban.ru/logo.jpg)

# Vue Bottom Sheet Vue 2
![Size](https://img.shields.io/bundlephobia/minzip/@webzlodimir/vue-bottom-sheet-vue2)
![Downloads](https://img.shields.io/npm/dt/@webzlodimir/vue-bottom-sheet-vue2)
![Version](https://img.shields.io/npm/v/@webzlodimir/vue-bottom-sheet-vue2)

A nice clean and touch-friendly bottom sheet component based on [Vue.js 2](https://vuejs.org/) and [hammer.js](https://hammerjs.github.io/) for Vue 2

- [Vue 3 Version](https://github.com/vaban-ru/vue-bottom-sheet)
- :clapper: [Demo](https://bs2.vaban.ru/)
- [中文文档](https://github.com/vaban-ru/vue-bottom-sheet-vue2/blob/master/README_CN.MD)

## Installation

### NPM

```
npm install @webzlodimir/vue-bottom-sheet-vue2
```

### Yarn

```
yarn add @webzlodimir/vue-bottom-sheet-vue2
```

## Usage

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
  import  VueBottomSheetVue2 from "@webzlodimir/vue-bottom-sheet-vue2";

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

| Prop                          | Type    | Description                                                                                                                            | Example                        | Defaults   |
|-------------------------------|---------|----------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|------------|
| max-width                     | Number  | Set max-width of component card in px                                                                                                  | `:max-width="640"`             | 640        |
| max-height                    | Number  | Sets the maximum height of the window, if not set it takes the height of the content                                                   | `:max-height="90"`             | -          |
| can-swipe                     | Boolean | Enable or disable swipe to close                                                                                                       | `:can-swipe="false"`           | true       |
| overlay                       | Boolean | Enable overlay                                                                                                                         | `:overlay="false"`             | true       |
| overlay-color                 | String  | Set overlay color with opacity                                                                                                         | `overlay-color="#0000004D"`    | #0000004D  |
| overlay-click-close           | Boolean | Close window on overlay click                                                                                                          | `:overlay-click-close="false"` | true       |
| close-height-percent `v0.0.7` | Number  | Set the percentage of the height in the Off state to the height in the fully enabled state. 100 indicates that the height is fully off | `:close-height-percent="50"`   | true       |
| init-sheet-height `v0.0.8`    | Number  | Sets the initial height of the window, if not set it takes the height of the content                                                   | `:init-sheet-height="300"`     | -          |
| z-index `v0.0.8`              | Number  | Set z-index of component card                                                                                                          | `:z-index="1001"`              | 99999      |
| custom-class `v0.0.8`         | String  | The component custom class name                                                                                                        | `custom-class="custom-style"`  | ''         |
| drag-color `v0.0.8`           | String  | Drag the bar icon color                                                                                                                | `drag-color="#ffc107"`         | #333333    |
| transition-duration           | Number  | Transition animation duration                                                                                                          | `:transition-duration="0.5"`   | 0.5        |

## Events

| Event         | Description                               | Example               |
|---------------|-------------------------------------------|-----------------------|
| opened        | Fire when card component is opened        | @opened=""            |
| closed        | Fire when card component is closed        | @closed=""            |
| dragging-up   | Fire while card component dragging up     | @dragging-up=""       |
| dragging-down | Fire while card component dragging down   | @dragging-down=""     |

## Slots

You can use this named slots:


| Slot          | Description       | 
|---------------|-------------------|
| drag `v0.0.8` | Drag the bar icon | 
| header        | header content    | 
| default       | main content      |
| footer        | footer content    |

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
