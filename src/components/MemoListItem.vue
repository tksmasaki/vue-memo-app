<template>
  <li>
    <a href="#" @click.prevent="$emit('edit-memo', memo)">
      {{ memoTitle }}
    </a>
  </li>
</template>

<script>
export default {
  name: 'MemoListItem',
  props: {
    memo: {
      type: Object,
      required: true
    }
  },
  computed: {
    memoTitle() {
      return this.memo.text.match(/.*/)[0]
    }
  }
}
</script>

<style scoped>
li {
  font-size: 1.2rem;
  line-height: 1.7;
}
</style>
