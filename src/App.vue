<template>
  <div id="app">
    <header>
      <h1>Memo App</h1>
    </header>
    <MemoList />
  </div>
</template>

<script>
import MemoList from './components/MemoList.vue'

export default {
  name: 'App',
  components: { MemoList },
  beforeCreate() {
    if (localStorage.getItem('nextMemoId') === null) {
      localStorage.setItem('nextMemoId', 1)
    }
  }
}
</script>

<style>
@import './css/reset.css';

*,
*:before,
*:after {
  box-sizing: border-box;
}
#app {
  max-width: 600px;
  margin: 0 auto;
}
header {
  margin: 1.5rem;
}
h1 {
  text-align: center;
  font-size: 2.5rem;
}
</style>
