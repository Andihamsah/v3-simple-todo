<template>
  <div class="container" style="margin-top: 20px">
    <div class="card">
      <div class="card-body">
        <h5 class="card-title">Simple Todo App</h5>
        <div class="row">
          <div class="col-md-10">
            <input v-model="todo" @keyup.enter="add" type="text" class="form-control" required/>
          </div>
          <div class="col-md-2">
            <button class="btn btn-success" @click="add">Add</button>
          </div>
        </div>
        <list :todos="list" @deleteTodo="deleteTodo" @doneTodo="doneTodo" />
        <small>Total Todo: {{ totalTodo }}</small>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, reactive, toRefs, onMounted, computed } from "vue";
import List from "./components/List.vue"
export default {
  components: { List },
  setup() {
    const todo = ref("");
    const todos = reactive({
      list: [],
    });
    
    onMounted(() => {
      const items = localStorage.getItem('todos');
      todos.list = items ? JSON.parse(items) : [];
    });

    const totalTodo = computed(() => {
      return todos.list.length;

    }) 

    const add = () => {
      todos.list.unshift({
        activity: todo.value,
        isDone: false
      });
      todo.value = "";
      saveToLocalStorage()
    };
    const deleteTodo = (todoIndex) => {
      todos.list = todos.list.filter((item, index) => {
        if (index != todoIndex) {
          return item
        }
      })
      saveToLocalStorage()

    }
    const doneTodo = (todoIndex) => {
      todos.list = todos.list.filter((item, index) => {
        if (index == todoIndex) {
          item.isDone = !item.isDone
        }

        return item;
      });
      saveToLocalStorage();
    };
    const saveToLocalStorage =  () =>  {
      localStorage.setItem('todos', JSON.stringify(todos.list))
    }

    return { todo, ...toRefs(todos), totalTodo, add, deleteTodo, doneTodo};
},
  // methods: {

  // },
  // methods: {
  //   kalihitung() {
  //     this.hitung *= 2
  //   }
  // }
};
</script>

<style scoped>
</style>

