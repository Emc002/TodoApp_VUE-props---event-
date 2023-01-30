<template>
  <div>
    <input type="text" class="todo-input" placeholder="What needs to be done" v-model="newTodo" @keyup.enter="addTodo">
    <transition-group name="fade" enter-active-class="animated fadeInUp" leave-active-class="animated fadeOutDown">

      <todo-item v-for="(todo, index) in todoFiltered" :key="todo.id" :todo="todo" :index="index" :checkAll="!anyRemaining"  @removedTodo="removeTodo" @finishedEdit="finishedEdit">
      </todo-item>
    </transition-group>

    <div class="extra-container">
      <div><label><input type="checkbox" @change="checkAllTodos" :checked="!anyRemaining">Check All</label></div>
      <div> {{ remaining }} item left</div>
    </div>

    <div class="extra-container">
      <button :class="{ active: filter == 'all' }" @click="filter = 'all'"> ALL </button>
      <button :class="{ active: filter == 'active' }" @click="filter = 'active'"> ACTIVE </button>
      <button :class="{ active: filter == 'completed' }" @click="filter = 'completed'"> COMPLETED </button>
    </div>

    <div>
      <transition name="fade">
        <button v-if="showClearCompletedButton" @click="clearCompleted">CLEAR COMPLETED</button>
      </transition>

    </div>
  </div>

</template> 

<script>
import TodoItem from './TodoItem.vue'
export default {
  name: "Todolist-app",
  components:{
    TodoItem,
  },
  data() {
    return {
      newTodo: "",
      idForTodo: 3,
      filter: 'all',
      todos: [
        {
          'id': 1,
          'title': "finish screens vue",
          'completed': false,
          'editing': false,

        },
        {
          'id': 2,
          'title': "take over the world",
          'completed': false,
          'editing': false,

        },
      ]
    }
  },
  computed: {
    remaining() {
      return this.todos.filter(todo => !todo.completed).length
    },

    anyRemaining() {
      return this.remaining != 0
    },

    todoFiltered() {
      if (this.filter == 'all') {
        return this.todos
      }
      else if (this.filter == 'active') {
        return this.todos.filter(todo => !todo.completed)
      }
      else if (this.filter == 'completed') {
        return this.todos.filter(todo => todo.completed)
      }
      return this.todos
    },
    showClearCompletedButton() {
      return this.todos.filter(todo => todo.completed).length > 0
    },

    clearCompleted() {
      this.todos = this.todos.filter(todo => !todo.completed)
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
      })

      this.newTodo = ''
      this.idForTodo++
    },

    removeTodo(index) {
      this.todos.splice(index, 1);

    },
    checkAllTodos() {
      this.todos.forEach((todo) => todo.completed =
        event.target.checked)
    },
    finishedEdit(data){
      this.todos.splice(data.index, 1 , data.todo)
    }

  }
}
</script>

<style lang="scss" >
@import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.5.2/animate.min.css");

.todo-input {
  width: 100%;
  padding: 10px 18px;
  font-size: 18px;
  margin-bottom: 16px;
}

.todo-item {
  margin-bottom: 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  animation-duration: 300ms;
}

.remove-item {
  cursor: pointer;
  margin-left: 14px;

  &:hover {
    color: rgb(255, 0, 0);
  }
}

.todo-item-left {
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
  border: 1px solid #ccc;
  font-family: Impact, Haettenschweiler, 'Arial Narrow Bold', sans-serif;
}

.completed {
  text-decoration: line-through;
  color: rgb(0, 255, 89);
}

.extra-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 16px;
  border-top: 1 px solid black;
  padding-top: 14px;
  margin-bottom: 14px;
}

button {
  font-size: 14px;
  background-color: #f2f2f2;
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

//CSS TRANSITION
.fade-enter-active,
.fade-leave-active {
  transition: opacity .2s;
}

.fade-enter,
.fade-leave {
  opacity: 0;
}
</style>
