<template>
  <div class="thispage">
    <h1>투두리스트</h1>
    <div class="category">
      <button @click="showCategory('all')" :class="{ 'selected': nowView === 'all' }">모두</button>
      <button @click="showCategory('active')" :class="{ 'selected': nowView === 'active' }">진행중</button>
      <button @click="showCategory('completed')" :class="{ 'selected': nowView === 'completed' }">완료됨</button>
    </div>
    <div class="container">
      <form class="content" @submit.prevent="addTodo">
        <input type="text" placeholder="여기에 입력하세요..." style="width: 400px; height: 25px;" v-model="newTodo" />
        <button style="background-color: #090;" type="submit">추가</button>
      </form>
      <template v-for="i in list.length" :key="i">
        <div v-if="showCondition(i)" class="content">
          <input type="checkbox" style="width: 25px; height: 25px;" v-model="listChecked[i - 1]" />
          <span :class="{ linethrough: listChecked[i - 1] }">{{ list[i - 1] }}</span>
          <button style="background-color: #08f;" @click="editTodo(i)">수정</button>
          <button style="background-color: #f77;" @click="removeTodo(i)">삭제</button>
        </div>
      </template>
    </div>
    <button class="removeall" @click="removeAll">모두 삭제</button>
  </div>
</template>

<style>
button {
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
}

.thispage {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 100px;
}

.category {
  display: flex;
  justify-content: space-between;
  width: 500px;
}

.category button {
  flex-grow: 1;
  width: 0;
  height: 30px;
  border-radius: 5px 5px 0 0;
  text-align: center;
  background-color: #f77;
  color: #fff;
  cursor: pointer;
  border: none;
}

.selected {
  background-color: #08f !important;
}

.container {
  width: 500px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.content {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 500px;
  margin: 5px;
}

.content span {
  width: 200px;
  height: 30px;
  border-radius: 5px;
  text-align: center;
  white-space: nowrap;
  overflow: hidden;
}

.content button {
  cursor: pointer;
  border: none;
  border-radius: 5px;
  width: 100px;
  height: 30px;
  color: #fff;
}

.linethrough {
  text-decoration: line-through;
}

.removeall {
  cursor: pointer;
  background-color: #f33;
  border: none;
  border-radius: 5px;
  width: 500px;
  height: 30px;
  color: #fff;
}
</style>

<script>
export default {
  name: 'App',
  data() {
    return {
      nowView: 'all',
      list: ['example1', 'example2', 'example3'],
      listChecked: [false, true, false],
    };
  },
  methods: {
    addTodo() {
      if (!this.newTodo) return;
      this.list.push(this.newTodo);
      this.listChecked.push(false);
      this.newTodo = '';
    },
    removeTodo(i) {
      this.list.splice(i - 1, 1);
      this.listChecked.splice(i - 1, 1);
    },
    showCategory(category) {
      this.nowView = category;
    },
    editTodo(i) {
      const newTodo = prompt('수정할 내용을 입력하세요...', this.list[i - 1]);
      if (newTodo) this.list[i - 1] = newTodo;
    },
    removeAll() {
      if (this.nowView === 'active'){
        this.list = this.list.filter((_, i) => this.listChecked[i]);
        this.listChecked = this.listChecked.filter((_, i) => this.listChecked[i]);
      } else if (this.nowView === 'completed') {
        this.list = this.list.filter((_, i) => !this.listChecked[i]);
        this.listChecked = this.listChecked.filter((_, i) => !this.listChecked[i]);
      } else {
        this.list = [];
        this.listChecked = [];
      }
    },
    showCondition(i) {
      if (this.nowView === 'all') return true;
      if (this.nowView === 'active' && !this.listChecked[i - 1]) return true;
      if (this.nowView === 'completed' && this.listChecked[i - 1]) return true;
      return false;
    },
  }
}
</script>
