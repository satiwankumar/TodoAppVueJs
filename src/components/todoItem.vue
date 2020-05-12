<template>
   <!-- <div class="todo-item" v-for="todo in todosFiltered" :key="todo.id"> -->
        <div  class="todo-item">
          <div class="todo-item-left">
            <input type="checkbox" v-model="completed" @change="doneEditTodo" />
            <div
              v-if="!editing"
              class="todo-item-label"
              @dblclick="editTodo"
              :class="{completed : completed}"
            >{{ title }}</div>

            <input
              v-else
              class="todo-item-edit"
              @blur="doneEditTodo"
              @keyup.enter="doneEditTodo"
              @keyup.esc="cancelEdit"
              type="text"
              v-model="title"
            />
          </div>
          <div class="remove-item" @click="removeTodo(index)">&times;</div>
        </div>
</template>

<script>
export default {
    name:'todo-item',

    props:{
        todo:{
            type : Object,
            required :true
        }, 
        index:{
            type:Number,
            required : true
        },
        checkAll:{
        
            type:Boolean,
            required:true
        }
    },
    data(){
        return{
            'id':this.todo.id,
            'title':this.todo.title,
            'completed':this.todo.completed,
            'editing':this.todo.editing,
            'beforeEditCache':''
        }
    },
    watch:{
        checkAll(){
        if(this.checkAll){
            this.completed = true

        }else{
            this.completed = this.todo.completed
        }
        }
        ,
        directives:{
            focus:{
                inserted: function(el){
                    el.focus()
                }
            }
        }

    },
    methods:{
        removeTodo(index){
            this.$emit('removeTodo',index)
        },
    editTodo() {
      this.beforeEditCache = this.title;
      // alert("double clicked")
      this.editing = true;
    },
    cancelEdit() {
      this.title = this.beforeEditCache;
      this.editing = false;
    },
     doneEditTodo() {
      if (this.title.trim() == 0) {
        this.title = this.beforeEditCache;
      }
      this.editing = false;
      this.$emit('finishedEdit',{
          'index' : this.index,
          'todo':{
              'id':this.id,
              'title':this.title,
              'completed':this.completed,
              'editing':this.editing
          } 
      })
    }
    }
}
</script>

<style>

</style>