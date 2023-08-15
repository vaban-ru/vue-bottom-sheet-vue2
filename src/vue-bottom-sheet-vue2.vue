<template>
  <div class="bottom-sheet" :style="{zIndex: zIndex}" ref="bottomSheet" :aria-hidden="!showSheet" role="dialog">
    <transition name="fade">
      <div
          :style="{ backgroundColor: overlayColor }"
          @click="clickOnOverlayHandler"
          class="bottom-sheet__overlay"
          v-if="overlay && showSheet"
      />
    </transition>
    <div
        ref="bottomSheetContent"
        :style="{
        maxWidth: maxWidthString,
        maxHeight: maxHeightString,
        transform: translateValueString,
        height: sheetHeightString
      }"
        :class="sheetContentClasses"
    >
      <header ref="bottomSheetHeader" class="bottom-sheet__header">
        <div class="bottom-sheet__draggable-area" ref="bottomSheetDraggableArea">
          <slot name="drag">
            <div class="bottom-sheet__draggable-thumb" :style="{backgroundColor: dragColor}"></div>
          </slot>
        </div>
        <slot name="header" />
      </header>
      <main ref="bottomSheetMain" class="bottom-sheet__main" :style="{overflowY: showSheet ? 'auto' : 'hidden'}">
        <slot />
      </main>
      <footer ref="bottomSheetFooter" class="bottom-sheet__footer">
        <slot name="footer" />
      </footer>
    </div>
  </div>
</template>

<script>
import Hammer from 'hammerjs'
export default {
  name: 'VueBottomSheetVue2',
  props: {
    overlay: {
      type: Boolean,
      default: true
    },
    overlayColor: {
      type: String,
      default: '#0000004D'
    },
    maxWidth: {
      type: Number,
      default: 640
    },
    maxHeight: {
      type: Number,
      default: undefined
    },
    overlayClickClose: {
      type: Boolean,
      default: true
    },
    canSwipe: {
      type: Boolean,
      default: true
    },
    closeHeightPercent: {
      type: Number,
      default: 100,
    },
    initSheetHeight: {
      type: Number,
      default: undefined
    },
    zIndex: {
      type: Number,
      default: 99999
    },
    customClass:{
      type: String,
      default: ''
    },
    dragColor: {
      type: String,
      default: '#333333'
    }
  },
  data() {
    return {
      showSheet: false,
      translateValue: this.closeHeightPercent,
      isDragging: false,
      contentScroll: 0,
      sheetHeight: 0
    }
  },
  methods: {
    initHeight() {
      this.sheetHeight = this.initSheetHeight ??
          (this.$refs.bottomSheetHeader.offsetHeight +
          this.$refs.bottomSheetMain.offsetHeight +
          this.$refs.bottomSheetFooter.offsetHeight)
    },
    clickOnOverlayHandler() {
      if (this.overlayClickClose) {
        this.close()
      }
    },
    dragHandler(event, type) {
      if (this.canSwipe) {
        this.isDragging = true

        const preventDefault = (e) => {
          e.preventDefault()
        }

        if (type === 'main') {
          this.contentScroll = this.$refs.bottomSheetMain.scrollTop
          document.documentElement.style.overflowY = 'hidden'
          document.documentElement.style.overscrollBehavior = 'none'
        }

        if (this.showSheet) {
          if (event.deltaY > 0) {
            if (type === 'main' && event.type === 'panup') {
              this.translateValue = this.pixelToVh(event.deltaY)
              if (event.cancelable) {
                this.$refs.bottomSheetMain.addEventListener('touchmove', preventDefault)
              }
            }

            if (type === 'main' && event.type === 'pandown' && this.contentScroll === 0) {
              this.translateValue = this.pixelToVh(event.deltaY)
            }

            if (type === 'area') {
              this.translateValue = this.pixelToVh(event.deltaY)
            }

            if (event.type === 'panup') {
              this.$emit('dragging-up')
            }
            if (event.type === 'pandown') {
              this.$emit('dragging-down')
            }
          }
        } else {
          if (type === 'main' && event.type === 'panup') {
            if (event.cancelable) {
              this.$refs.bottomSheetMain.addEventListener('touchmove', preventDefault)
            }
            let tslVal = this.closeHeightPercent + this.pixelToVh(event.deltaY)
            if (tslVal >= 0) {
              this.translateValue = tslVal
            }
          }
          if (type === 'main' && event.type === 'pandown' && this.contentScroll === 0) {
            this.translateValue = this.closeHeightPercent + this.pixelToVh(event.deltaY)
          }

          if (type === 'area') {
            let tslVal = this.closeHeightPercent + this.pixelToVh(event.deltaY)
            if (tslVal >= 0) {
              this.translateValue = tslVal
            }
          }

          if (event.type === 'panup') {
            this.$emit('dragging-up')
          }
          if (event.type === 'pandown') {
            this.$emit('dragging-down')
          }
        }

        if (event.isFinal) {
          this.$refs.bottomSheetMain.removeEventListener('touchmove', preventDefault)

          if (type === 'main') {
            this.contentScroll = this.$refs.bottomSheetMain.scrollTop
          }
          this.isDragging = false
          if (this.showSheet) {
            if ((this.pixelToVh(event.deltaY) >= 15 && this.contentScroll === 0) || (this.pixelToVh(event.deltaY) >= 15 && type === 'area')) {
              this.close()
            } else {
              this.open()
            }
          } else {
            if (this.pixelToVh(event.deltaY) <= -5) {
              this.open()
            } else {
              this.close()
            }
          }
        }
      }
    },
    pixelToVh(pixel) {
      const height =
          this.maxHeight && this.maxHeight <= this.sheetHeight ? this.maxHeight : this.sheetHeight
      return (pixel / height) * 100
    },
    close() {
      this.showSheet = false
      this.translateValue = this.closeHeightPercent
      setTimeout(() => {
        document.documentElement.style.overflowY = 'auto'
        document.documentElement.style.overscrollBehavior = ''
        this.$emit('closed')
      }, this.transitionDuration * 1000)
    },
    open() {
      this.translateValue = 0
      document.documentElement.style.overflowY = 'hidden'
      document.documentElement.style.overscrollBehavior = 'none'
      this.showSheet = true
      this.$emit('opened')
    },
    keyupHandler(event){
      const isFocused = (element) => document.activeElement === element
      const isSheetElementFocused =
          this.$refs.bottomSheet.contains(event.target) && isFocused(event.target)

      if (event.key === 'Escape' && !isSheetElementFocused) {
        this.close()
      }
    },
  },
  beforeDestroy() {
    window.removeEventListener('keyup', this.keyupHandler)
  },
  async mounted() {
   setTimeout(() =>{
     this.initHeight()

     window.addEventListener('keyup', this.keyupHandler)

     /**
      * Create instances of Hammerjs
      */
     const hammerAreaInstance = new Hammer(this.$refs.bottomSheetDraggableArea, {
       inputClass: Hammer.TouchMouseInput,
       recognizers: [[Hammer.Pan, { direction: Hammer.DIRECTION_VERTICAL }]]
     })

     const hammerMainInstance = new Hammer(this.$refs.bottomSheetMain, {
       inputClass: Hammer.TouchMouseInput,
       recognizers: [[Hammer.Pan, { direction: Hammer.DIRECTION_VERTICAL }]]
     })

     /**
      * Set events and handlers to hammerjs instances
      */
     hammerAreaInstance.on('panstart panup pandown panend', (e) => {
       this.dragHandler(e, 'area')
     })

     hammerMainInstance.on('panstart panup pandown panend', (e) => {
       this.dragHandler(e, 'main')
     })
   }, 100)
  },
  watch: {
    initSheetHeight(newVal, oldVal){
      this.initHeight()
    }
  },
  computed: {
    sheetContentClasses() {
      return [
        'bottom-sheet__content',
        {
          'bottom-sheet__content--fullscreen': this.sheetHeight >= window.innerHeight,
          'bottom-sheet__content--dragging': this.isDragging
        },
        this.customClass
      ]
    },
    maxWidthString() {
      return `${this.maxWidth}px`
    },
    maxHeightString() {
      return this.maxHeight ? `${this.maxHeight}px` : 'inherit'
    },
    translateValueString() {
      return `translate3d(0, ${this.translateValue}%, 0)`
    },
    sheetHeightString() {
      return this.sheetHeight && this.sheetHeight > 0 ? `${this.sheetHeight + 1}px` : 'auto'
    }
  }
}
</script>

<style lang="scss" scoped>
.bottom-sheet {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-end;

  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  transition: visibility 0.5s;

  * {
    box-sizing: border-box;
  }

  &[aria-hidden='false'] {
    visibility: visible;
  }

  &[aria-hidden='true'] {
    //visibility: hidden;
    pointer-events: none;
  }

  &__overlay {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    z-index: -1;
  }

  &__content {
    display: flex;
    flex-direction: column;
    border-radius: 16px 16px 0 0;
    background: #ffffff;
    overflow-y: hidden;
    box-sizing: border-box;
    pointer-events: all;
    width: 100%;

    &--fullscreen {
      border-radius: 0;
    }

    &:not(.bottom-sheet__content--dragging) {
      transition: 0.5s ease;
    }
  }

  &__draggable-area {
    width: 100%;
    margin: auto;
    padding: 16px;
    cursor: grab;
  }

  &__draggable-thumb {
    width: 40px;
    height: 4px;
    background: #333333;
    border-radius: 8px;
    margin: 0 auto;
  }

  &__main {
    display: flex;
    flex-direction: column;
    overflow-y: scroll;
    box-sizing: border-box;
    -webkit-overflow-scrolling: touch;
    touch-action: auto !important;

    &::-webkit-scrollbar {
      height: 8px;
      width: 8px;
    }
    &::-webkit-scrollbar-corner {
      display: none;
    }
    &:hover::-webkit-scrollbar-thumb {
      background-color: rgba(0, 0, 0, 0.2);
      border-radius: 8px;
    }
    &::-webkit-scrollbar-thumb {
      background-color: rgba(0, 0, 0, 0);
    }
  }

  &__footer:empty {
    display: none;
  }
}

.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to  {
  opacity: 0;
}
</style>
