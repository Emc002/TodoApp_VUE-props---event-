<template>
  <div class="todo-item">

    <div class="todo-item-left">
      <input type="checkbox" v-model="completed" @change="doneEdit">
      <div v-if="!editing" @dblclick="editTodo" :class="{ completed:completed }"
        class="todo-item-label">{{title }}</div>
      <input v-else type="text" class="todo-item-edit"  v-model="title"
        @blur="doneEdit" @keyup.esc="cancelEdit" @keyup.enter="doneEdit" v-focus>

    </div>
    <div class="remove-item" @click="removeTodo(index)">
      &times;
    </div>

  </div>
</template>

<script>
export default {
  name: 'todo-item',
  props:  {
    todo: {
      type: Object,
      required: true,
    },
    index: {
      type: Number,
      required: true,
    },
    checkAll: {
      type: Boolean,
      required: true,
    }
  },

  data() {
    return {
      'id':this.todo.id,
      'title':this.todo.title,
      'completed':this.todo.completed,
      'editing':this.todo.editing,
      'beforeEditCache': ''
    }
  },
  watch: {
    checkAll(){
      this.completed = this.checkAlls ? true : this.todo.completed
    }
  },
  directives: {
    focus: {
      inserted: function (el) {
        el.focus()
      }
    }
  },
  methods: {
    removeTodo(index){
      this.$emit('removedTodo',index)
    },
    editTodo(){
      this.beforeEditCache = this.title
      this.editing = true
    },
    cancelEditTodo() {
      this.title = this.beforeEditCache
      this.editing = false
    },

    doneEdit() {
      if (this.title.trim() == '') {
        this.title = this.beforeEditCache
        
      }
      this.editing = false

      this.$emit('finishedEdit',{
        'index': this.index,
        'todo':{
          'id': this.id,
          'title': this.title,
          'completed':this.completed,
          'editing': this.editing

        }
      })
    },
    cancelEdit() {
      this.title = this.beforeEditCache
      this.editing = false
    },

  }
}
</script>