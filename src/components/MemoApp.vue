<template>
  <div class="memo-app">
      <memo-form @addMemo="addMemo" />
      <ul class="memo-list">
        <memo v-for="memo in memos" :key="memo.id" :memo="memo" @deleteMemo="deleteMemo" @updateMemo="updateMemo" />
      </ul>
  </div>
</template>

<script>
import MemoForm from './MemoForm';
import Memo from './Memo';

export default {
  name: 'MemoApp',
  data () {
      return {
          memos: [],
      }
  }, 
  created() {
      this.memos = localStorage.memos ? JSON.parse(localStorage.memos) : [];
  },
  components: {
    'MemoForm': MemoForm, 
    'Memo': Memo
  }, 
  methods: {
    addMemo (payload) {
      this.memos.push(payload);
      this.storeMemo();
    }, 
    storeMemo () {
      const memoToString = JSON.stringify(this.memos);
      localStorage.setItem('memos', memoToString);
    }, 
    deleteMemo (id) {
      const targetIndex = this.memos.findIndex(v => v.id === id);
      this.memos.splice(targetIndex, 1);
      this.storeMemo();
    }, 
    updateMemo (payload) {
      const { id, content } = payload;
      const targetIndex = this.memos.findIndex(v => v.id === id);
      const targetMemo = this.memos[targetIndex];
      this.memos.splice(targetIndex, 1, { ...targetMemo, content });
      this.storeMemo();
    }
  }
}
</script>

<style>
.memo-list {
  padding: 20px 0;
  margin: 0;
}
</style>
