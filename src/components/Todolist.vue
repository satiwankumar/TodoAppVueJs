<template>
  <div>
    <input
      type="text"
      class="todo-input"
      placeholder="What needs to be done"
      v-model="newTodo"
      @keyup.enter="addTodo"
    />
    <transition-group
      name="fade"
      enter-active-class="animated fadeInUp"
      leave-active-class="animated fadeOutDown"
    >
      <todoItem
        v-for="(todo,index) in todosFiltered"
        :key="todo.id"
        :todo="todo"
        :index="index"
        @removeTodo="removeTodo"
        @finishedEdit="finishedEdit"
        :checkAll="!anyRemaining"
      >
        <!-- <div class="todo-item-left">
            <input type="checkbox" v-model="todo.completed" />
            <div
              v-if="!todo.editing"
              class="todo-item-label"
              @dblclick="editTodo(todo)"
              :class="{completed : todo.completed}"
            >{{ todo.title }}</div>

            <input
              v-else
              class="todo-item-edit"
              @blur="doneEditTodo(todo)"
              @keyup.enter="doneEditTodo(todo)"
              @keyup.esc="cancelEdit(todo)"
              type="text"
              v-model="todo.title"
            />
          </div>
        <div class="remove-item" @click="removeTodo(index)">&times;</div>-->
      </todoItem>
    </transition-group>

    <div class="extra-container">
      <div>
        <label>
          <input :checked="!anyRemaining" @change="checkAllTodos" type="checkbox" />
          check all
        </label>
      </div>

      <div>{{remaining}} itemsleft</div>
    </div>
    <div class="extra-container">
      <div>
        <button :class="{active : filter =='all'}" @click="filter = 'all'">All</button>
        <button :class="{active : filter =='active'}" @click="filter = 'active'">Active</button>
        <button :class="{active : filter =='completed'}" @click="filter = 'completed'">Completed</button>

        <transition name="fade">
          <button v-if="showClearCompletedButton" @click="clearCompleted()">clearCompleted</button>
        </transition>
      </div>
    </div>
  </div>
</template>

<script>
import todoItem from "./todoItem";
export default {
  name: "TodoList",
  components: {
    todoItem
  },
  data() {
    return {
      newTodo: "",
      idForTodo: 3,
      beforeEditCache: "",
      filter: "all",
      todos: [
        {
          id: 1,
          title: "Freelance",
          completed: false,
          editing: false
        },
        {
          id: 2,
          title: "Final year project",
          completed: false,
          editing: false
        }
      ]
    };
  },
  computed: {
    //computed property is for computing new data from existing data
    //they should not mutatte data
    //they should not accept parameters
    //they should always return something

    remaining() {
      return this.todos.filter(todo => !todo.completed).length;
    },
    anyRemaining() {
      return this.remaining != 0;
    },
    todosFiltered() {
      if (this.filter == "all") {
        return this.todos;
      } else if (this.filter == "active") {
        return this.todos.filter(todo => !todo.completed);
      } else if (this.filter == "completed") {
        return this.todos.filter(todo => todo.completed);
      }
      return this.todos;
    },
    showClearCompletedButton() {
      return this.todos.filter(todo => todo.completed).length > 0;
    }
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim().length == 0) {
        return;
      }
      this.todos.push({
        id: this.idForTodo,
        title: this.newTodo,
        completed: false,
        editing: false
      });
      (this.newTodo = ""), this.idForTodo++;
    },
    removeTodo(index) {
      this.todos.splice(index, 1);
    },
    editTodo(todo) {
      this.beforeEditCache = todo.title;
      // alert("double clicked")
      todo.editing = true;
    },
    cancelEdit(todo) {
      todo.title = this.beforeEditCache;
      todo.editing = false;
    },
    doneEditTodo(todo) {
      if (todo.title.trim() == 0) {
        todo.title = this.beforeEditCache;
      }
      todo.editing = false;
    },
    checkAllTodos() {
      this.todos.forEach(todo => (todo.completed = event.target.checked));
    },
    clearCompleted() {
      this.todos = this.todos.filter(todo => !todo.completed);
    },
    finishedEdit(data) {
      this.todos.splice(data.index, 1, data.todo);
    }
  }
};
</script>


<style lang="scss">
@import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.5.2/animate.min.css");
.todo-input {
  width: 100%;
  padding: 10px 18px;
  font-size: 18px;
  margin-bottom: 16px;
  &:focus {
    outline: 0;
  }
}
.todo-item {
  margin-bottom: 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  animation-duration: 0.3s;
}
.remove-item {
  cursor: pointer;
  margin-left: 14px;
  &:hover {
    color: black;
  }
}
.todo-item-left {
  // later
  display: flex;
  align-items: center;
}
.todo-item-label {
  padding: 10px;
  border: 1px solid white;
  margin-left: 12px;
}
.todo-item-edit {
  font-size: 24px;
  color: #2c3e50;
  margin-left: 12px;
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc; //override defaults
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  &:focus {
    outline: none;
  }
}
.completed {
  text-decoration: line-through;
  color: grey;
}
.extra-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 16px;
  border-top: 1px solid lightgrey;
  padding-top: 14px;
  margin-bottom: 14px;
}
button {
  font-size: 14px;
  background-color: white;
  appearance: none;
  &:hover {
    background: lightgreen;
  }
  &:focus {
    outline: none;
  }
}
.active {
  background: lightgreen;
}
// CSS Transitions
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.2s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>