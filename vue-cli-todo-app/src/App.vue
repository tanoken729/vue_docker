<template>
  <div id="app">
    <div class="title">
      <h1>Todoリスト</h1>
    </div>
    <div class="main">
      <div class="switch">
        <ul class="show-list">
          <li v-for="item in items" :key="item.id">
            <input type="radio" name="show" :id="item.id" :value="item.value" v-model="show">
            <label :for="item.id">{{item.text}}</label>
          </li>
        </ul>
      </div>
      <div class="tasks">
        <table>
          <tr>
            <th>ID</th>
            <th>コメント</th>
            <th>状態</th>
          </tr>
          <tr v-for="(list, index) in showTasks" v-bind:key="index">
            <td>{{ list.id }}</td>
            <td>{{ list.comment }}</td>
            <td v-if="list.state"><button @click="changeTaskState(list.id)">作業中</button></td>
            <td v-else><button @click="changeTaskState(list.id)">完了</button></td>
            <td><button @click="deleteTask(list.id)">削除</button></td>
          </tr>
        </table>
      </div>
    </div>
    <div class="addTask">
      <h1>新規タスクの追加</h1>
        <div class="form">
          <form @submit.prevent>
              <input type="text" v-model="comment">
              <input type="submit" value="送信" @click="addTask">
          </form>
        </div>
    </div>
  </div>
</template>

<script>

export default {
  data() {
    return {
      items: [
        {id: 'all', value: 0, text: 'すべて'},
        {id: 'doing', value: 1, text: '作業中'},
        {id: 'done', value: 2, text: '完了'}
      ],
      lists: [],
      comment: '',
      show: 0,
    }
  },
  methods: {
    addTask: function(){
      const todo = {
        comment: this.comment,
        state: true,
        id: this.lists.length
      }
      this.lists.push(todo)
      this.comment = '';
    },
    deleteTask(list_id) {
      if (list_id > -1) {
      this.lists.splice(list_id, 1)
      this.lists.forEach((list, index) => { // 修正箇所： idの振り直し
          list.id = index;
        })
      }
    },
    changeTaskState(list_id) {
      //ボタンを押下したときlist_idの元の場所のレコードが反応してしまう
        this.lists[list_id].state = !this.lists[list_id].state
    },
  },
  computed: {
    showTasks() {
      switch(this.show) {
        case 0:
          return this.lists;
        // 作業中の場合stateがtrueのものだけ表示させる
        case 1:
          return this.lists.filter(e => e.state)
        // 完了の場合stateがfalseのものだけ表示させる
        case 2:
          return this.lists.filter(e => !e.state)
        default:
          return []
      }
    }
  }
};

</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}

.show-list {
  display: flex;
  margin: 5px 0;
}
</style>