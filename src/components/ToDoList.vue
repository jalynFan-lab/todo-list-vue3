<template>
  <div>
    <span>Filter:</span>
    <button @click="doneClick">Done</button>
    <button @click="todoClick">TODO</button>
  </div>
  <ul>
    <todo-item
      v-for="todo in filterTodoitems.arr"
      :key="todo.id"
      :done="todo.done"
      @toggle="todo.done = !todo.done"
    >
      {{ todo.text }}
    </todo-item>
    <input type="text" v-model="inputTodo" />
    <button @click="addClick">Add</button>
  </ul>

  <section class="alert alert-primary">
    <h1>{{ data.title }}</h1>
    <p>{{ data.message }}</p>

    <textarea v-model="data.mydata" rows="5" class="form-control"></textarea>
  </section>
</template>
g
<script>
import { ref, reactive, watch, watchEffect, onMounted } from "vue";
import axios from "axios";
import TodoItem from "./TodoItem.vue";

export default {
  name: "ToDoList",
  setup() {
    //元々のTODOリスト
    let todoItems = reactive([
      { id: 1, text: "Go to eat and swim,have a happy day", done: false },
      { id: 2, text: "Go shopping", done: false },
      { id: 3, text: "Go to the park", done: true },
    ]);

    const path = "/data.txt";
    const data = reactive({
      title: "Axios",
      message: "This is a axios message",
      mydata: "",
    });
    //handle add
    const inputTodo = ref("");

    const addClick = () => {
      todoItems.push({
        id: todoItems.length + 1,
        text: inputTodo.value,
        done: false,
      });

      inputTodo.value = "";
    };

    //filter
    //reactive 数组不能直接赋值
    let filterTodoitems = reactive({ arr: todoItems });
    let state = "";

    const doneClick = () => {
      state = "doneClick";
      filterTodoitems.arr = todoItems.filter((todo) => todo.done == true);
    };

    const todoClick = () => {
      state = "todoClick";
      filterTodoitems.arr = todoItems.filter((todo) => todo.done == false);
    };
    //#region
    // watch(todoItems, () => {
    //   console.log(state);
    //   if (state == "doneClick") {
    //     filterTodoitems.arr = todoItems.filter((todo) => todo.done == true);
    //   } else if (state == "todoClick") {
    //     filterTodoitems.arr = todoItems.filter((todo) => todo.done == false);
    //   } else {
    //     filterTodoitems.arr = todoItems;
    //   }
    // });
    //#endregion
    watchEffect(() => {
      if (state == "doneClick") {
        filterTodoitems.arr = todoItems.filter((todo) => todo.done == true);
      } else if (state == "todoClick") {
        filterTodoitems.arr = todoItems.filter((todo) => todo.done == false);
      } else {
        filterTodoitems.arr = todoItems;
      }
    });

    const getData = async () => {
      let res = await axios.get(path);
      data.mydata = res.data;
    };

    onMounted(() => {
      getData();
    });

    return {
      inputTodo,
      filterTodoitems,
      addClick,
      doneClick,
      todoClick,
      data,
      getData,
    };
  },
  components: {
    TodoItem,
  },
};
</script>
