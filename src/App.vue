<template>
  <div class="container mt-3">
    <div class="card">
      <div class="card-body">
        <div class="card-title">
          SIMPLE TODO APP
        </div>
        <div class="row">
          <div class="col-10">
            <input type="text" v-model="todo" class="form-control" @keyup.enter="add">
          </div>
          <div class="col-2">
            <button class="btn btn-success w-100" @click="add">ADD</button>
          </div>
        </div>
        <list :todos="todos.list" @deleteTodo="deleteTodo" @doneTodo="doneTodo"/>
        <div class="mt-2">
          Jumlah Todo : {{ jmlTodo }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import List from './components/List.vue'
import { ref,reactive, onMounted, computed} from 'vue'

export default{
  components:{
    List
  },
  setup(){
    const todo = ref("")
    
    const todos = reactive({
      list : []
    })
    
    const jmlTodo = computed(() => todos.list.length )

    onMounted(() => {
      const items = localStorage.getItem('todos')
      todos.list = items ? JSON.parse(items) : []
    })

    const add = () => {
      todos.list.unshift({
        act:todo.value,
        isDone:false
      })
      todo.value = ""
      saveToLocalStorage()
    }

    const deleteTodo = (indexTodo) => {
      todos.list = todos.list.filter((item, index) => {
        if(index != indexTodo){
          return item
        }
      })
      saveToLocalStorage()
    }

    const doneTodo = (indexTodo) =>{
      todos.list = todos.list.filter((item, index) => {
        if(index == indexTodo){
          if(item.isDone == true){
            item.isDone = false
          }else{
            item.isDone = true
          }
        }
        return item
      })
      saveToLocalStorage()
    }

    const saveToLocalStorage = () => {
      localStorage.setItem('todos',JSON.stringify(todos.list));
    }

    return {
      todo, todos, jmlTodo, add, deleteTodo, doneTodo
    }
  }
}
</script>
