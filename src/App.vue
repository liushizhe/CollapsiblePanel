<template>
  <div id="app">
    <div class="content">
      <div class="label">
        可收缩面板示例
      </div>
      <collapsible-panel
        left="120px"
        top="30px"
        width="50vw"
        height="200px"
        position="bottom">
        <h2>可收缩面板</h2>
        <p>这是一个可以通过按钮收缩的面板。</p>
        <p>点击箭头按钮可以收起或展开面板。</p>
      </collapsible-panel>
    </div>
    <router-view/>
  </div>
</template>

<script>
import CollapsiblePanel from './components/CollapsiblePanel.vue'

export default {
  name: 'App',
  components: {
    CollapsiblePanel
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

body {
  margin: 0;
}

.content {
  position: relative;
  margin: 0 auto;
  height: 100vh;
}

.label {
  position: absolute;
  left: 120px;
  top: 0;
  height: 30px;
  line-height: 30px;
  background: #f5f7fa;
  border: 1px solid #dcdfe6;
  border-radius: 4px;
  text-align: left;
  padding: 0 15px;
  box-sizing: border-box;
  z-index: 1;
  width: 50vw;
}
</style>
