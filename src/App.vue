<template>
  <div id="app">
    <Header></Header>
    <h1>
      My Todo Task
      <span class="info">({{remainingTask.length}}/{{todos.length}})</span>
      <button @click="deleteEndTask">完了タスクの削除</button>
    </h1>
    <p>
      <input type="text" v-model="comment" />
    </p>
    <p>{{comment}}</p>

    <ul>
      <li v-for="(todo,index) in todos" :key="todo.message">
        <input type="checkbox" v-model="todo.isDone" />
        <span :class="{done: todo.isDone}">{{todo.name}}</span>
        <span @click="deleteTask(index)" class="xButton">[x]</span>
      </li>
    </ul>
    <form @submit.prevent="addItem">
      <input type="text" v-model="newTask" />
      <input type="submit" value="追加" />
    </form>
  </div>
</template>

<script>
import Header from "./components/Header";
export default {
  components: {
    Header
  },
  name: "App",
  data() {
    return {
      comment: "",
      newTask: "",
      todos: [
        { name: "Task1", isDone: false },
        { name: "Task2", isDone: false },
        { name: "Task3", isDone: false },
        { name: "Task4", isDone: false }
      ]
    };
  },
  watch: {
    todos: {
      handler: function() {
        localStorage.setItem("todos", JSON.stringify(this.todos));
      },
      deep: true
    }
  },
  created: function() {
    this.todos = JSON.parse(localStorage.getItem("todos")) || [];
  },
  methods: {
    addItem: function() {
      console.log("addItem");
      this.todos.push({
        name: this.newTask,
        isDone: false
      });
      this.newTask = "";
    },
    deleteTask: function(index) {
      this.todos.splice(index, 1);
    },
    deleteEndTask: function() {
      this.todos = this.remainingTask;
    }
  },
  computed: {
    remainingTask: function() {
      return this.todos.filter(function(todo) {
        return !todo.isDone;
      });
    }
  }
};
</script>


<style scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
h1 {
  font-size: 16px;
  border-bottom: 1px solid #ddd;
  padding: 16px 0;
}

.xButton {
  cursor: pointer;
  font-size: 12px;
  color: red;
}

li > span.done {
  text-decoration: line-through;
  color: #bbb;
}

.info {
  color: #bbb;
  font-size: 12px;
}

li > span.done {
  text-decoration: line-through;
  color: #bbb;
}
</style>
