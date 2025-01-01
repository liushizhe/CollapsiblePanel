<template>
  <div class="collapsible-panel-container" 
       :class="{'is-collapsed': isCollapsed}"
       :style="containerStyle">
    <div class="panel-wrapper" 
         :class="`position-${position}`"
         :style="wrapperStyle">
      <div class="panel">
        <slot></slot>
      </div>
      <div class="collapse-button" 
           :class="[`position-${position}`, {'is-collapsed': isCollapsed}]"
           @click.stop="toggleCollapse">
        <i :class="getArrowClass()"></i>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CollapsiblePanel',
  props: {
    left: {
      type: String,
      default: '0'
    },
    top: {
      type: String,
      default: '0'
    },
    width: {
      type: String,
      default: '100%'
    },
    height: {
      type: String,
      default: 'auto'
    },
    position: {
      type: String,
      default: 'bottom',
      validator: value => ['top', 'right', 'bottom', 'left'].includes(value)
    }
  },
  data() {
    return {
      isCollapsed: false,
      panelSize: 0
    }
  },
  computed: {
    containerStyle() {
      const style = {
        top: this.top,
        width: this.width,
        height: this.height,
        left: this.left
      }

      // 根据位置调整容器的起始位置
      if (this.position === 'right') {
        style.transform = this.isCollapsed ? `translateX(-${this.width})` : 'translateX(0)'
        style.transformOrigin = 'right'
      } else if (this.position === 'top') {
        style.transform = this.isCollapsed ? `translateY(${this.height})` : 'translateY(0)'
        style.transformOrigin = 'top'
      }

      return style
    },
    wrapperStyle() {
      if (!this.isCollapsed) {
        return {
          transform: 'translate(0, 0)',
          pointerEvents: 'auto'
        }
      }
      
      let transform = ''
      switch (this.position) {
        case 'left':
          transform = `translateX(${this.panelSize}px)`
          break
        case 'right':
          transform = 'translateX(0)'
          break
        case 'top':
          transform = 'translateY(0)'
          break
        case 'bottom':
          transform = `translateY(-${this.panelSize}px)`
          break
        default:
          transform = 'translate(0, 0)'
      }

      return {
        transform,
        pointerEvents: 'none'
      }
    }
  },
  mounted() {
    // 获取面板尺寸（根据位置决定是高度还是宽度）
    const panel = this.$el.querySelector('.panel')
    if (this.position === 'left' || this.position === 'right') {
      this.panelSize = panel.offsetWidth
    } else {
      this.panelSize = panel.offsetHeight
    }
  },
  methods: {
    toggleCollapse() {
      this.isCollapsed = !this.isCollapsed
    },
    getTransform() {
      if (!this.isCollapsed) return 'translate(0, 0)'
      
      switch (this.position) {
        case 'left':
          return `translateX(${this.panelSize}px)`
        case 'right':
          return `translateX(-${this.panelSize}px)`
        case 'top':
          return `translateY(${this.panelSize}px)`
        case 'bottom':
          return `translateY(-${this.panelSize}px)`
        default:
          return 'translate(0, 0)'
      }
    },
    getArrowClass() {
      const baseClass = {
        'is-collapsed': this.isCollapsed
      }
      
      switch (this.position) {
        case 'left':
          baseClass['el-icon-arrow-right'] = true
          break
        case 'right':
          baseClass['el-icon-arrow-left'] = true
          break
        case 'top':
          baseClass['el-icon-arrow-down'] = true
          break
        case 'bottom':
          baseClass['el-icon-arrow-up'] = true
          break
      }
      
      return baseClass
    }
  }
}
</script>

<style scoped>
.collapsible-panel-container {
  position: absolute;
  z-index: 0;
  transition: all 0.3s ease;
}

.collapsible-panel-container.is-collapsed {
  pointer-events: none;
}

.panel-wrapper {
  position: relative;
  transition: transform 0.3s ease;
  display: flex;
  flex-direction: column;
  height: 100%;
}

.panel {
  background: #fff;
  border: 1px solid #dcdfe6;
  border-radius: 4px;
  padding: 20px;
  box-sizing: border-box;
  height: 100%;
}

.collapse-button {
  position: absolute;
  background: #fff;
  border: 1px solid #dcdfe6;
  border-radius: 4px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  z-index: 2;
  transition: all 0.3s ease;
  pointer-events: auto !important;
  width: 32px;
  height: 32px;
}

/* 底部按钮样式 */
.collapse-button.position-bottom {
  left: 50%;
  transform: translateX(-50%);
  top: 100%;
}

/* 顶部按钮样式 */
.collapse-button.position-top {
  left: 50%;
  transform: translateX(-50%);
  bottom: 100%;
}

/* 左侧按钮样式 */
.collapse-button.position-left {
  top: 50%;
  transform: translateY(-50%);
  right: 100%;
}

/* 右侧按钮样式 */
.collapse-button.position-right {
  top: 50%;
  transform: translateY(-50%);
  left: 100%;
}

.collapse-button:hover {
  background: #f5f7fa;
}

[class^="el-icon-arrow-"] {
  transition: transform 0.3s ease;
  font-size: 16px;
}

/* 各个位置的箭头旋转效果 */
.position-bottom .el-icon-arrow-up.is-collapsed {
  transform: rotate(180deg);
}

.position-top .el-icon-arrow-down.is-collapsed {
  transform: rotate(180deg);
}

.position-left .el-icon-arrow-right.is-collapsed {
  transform: rotate(180deg);
}

.position-right .el-icon-arrow-left.is-collapsed {
  transform: rotate(180deg);
}
</style>
