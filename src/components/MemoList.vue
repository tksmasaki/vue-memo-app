<template>
  <div class="flex-container">
    <div class="flex-item">
      <div class="memo-list-titles">
        <ul v-if="memos.length">
          <MemoListItem
            v-for="memo in memos"
            :key="memo.id"
            :memo="memo"
            @edit-memo="editMemo"
          />
        </ul>
        <p v-else>既存のメモはありません。</p>
      </div>
      <div class="symbol-of-add-memo">
        <a href="#" @click.prevent="addMemo">+</a>
      </div>
    </div>
    <div v-show="editable" class="memo-editor flex-item">
      <textarea v-model="editingText" cols="30" rows="30"></textarea>
      <div class="btn-wrapper">
        <button class="update-btn" @click="updateMemo">編集</button>
        <button class="delete-btn" @click="deleteMemo">削除</button>
      </div>
    </div>
  </div>
</template>

<script>
import MemoListItem from './MemoListItem.vue'

export default {
  name: 'MemoList',
  components: { MemoListItem },
  data() {
    return {
      memos: JSON.parse(localStorage.getItem('memos')) ?? [],
      nextMemoId: parseInt(localStorage.getItem('nextMemoId')),
      editingId: null,
      editingText: ''
    }
  },
  computed: {
    editable() {
      return this.editingId !== null
    }
  },
  methods: {
    addMemo() {
      const newMemo = { id: this.nextMemoId, text: '新規メモ' }
      this.memos.push(newMemo)
      localStorage.setItem('memos', JSON.stringify(this.memos))
      localStorage.setItem('nextMemoId', ++this.nextMemoId)
      this.editMemo(this.memos[this.findMemoIndexById(newMemo.id)])
    },
    editMemo(memo) {
      this.editingId = memo.id
      this.editingText = memo.text
    },
    quitEditMemo() {
      this.editingId = null
      this.editingText = ''
    },
    updateMemo() {
      this.memos.splice(this.findMemoIndexById(this.editingId), 1, {
        id: this.editingId,
        text: this.editingText
      })
      localStorage.setItem('memos', JSON.stringify(this.memos))
      this.quitEditMemo()
    },
    deleteMemo() {
      this.memos.splice(this.findMemoIndexById(this.editingId), 1)
      localStorage.setItem('memos', JSON.stringify(this.memos))
      this.quitEditMemo()
    },
    findMemoIndexById(memoId) {
      return this.memos.findIndex((memo) => memo.id === memoId)
    }
  }
}
</script>

<style scoped>
ul {
  list-style: none;
}
.flex-container {
  display: flex;
}
.flex-item {
  flex: 1;
}
.memo-list-titles {
  margin-right: 1rem;
}
.symbol-of-add-memo {
  margin: 1.5rem 1rem;
  margin-left: 0;
}
.symbol-of-add-memo a {
  text-decoration: none;
  font-size: 2.5rem;
}
.memo-editor textarea {
  width: 100%;
}
.btn-wrapper {
  width: 100%;
  display: flex;
}
.update-btn {
  flex: 3;
  margin: 1rem 0.5rem 1rem 0;
}
.delete-btn {
  flex: 1;
  margin: 1rem 0 1rem 0.5rem;
}
</style>
