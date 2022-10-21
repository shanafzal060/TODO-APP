<template>
  <div>
    <h1 class="mb-3">TODO APP</h1>
    <h3 class="name">
      what's up
      <input
        type="text"
        placeholder="Name here"
        class="fw-bold mb-2"
        v-model="userName"
      />
    </h3>
    <form @submit.prevent="addNewTodo">
      <input
        v-model="toDoInput"
        name="newTodo"
        type="text"
        id=""
        class="form-control form-control-lg w-50 h-100 m-auto"
        placeholder="e.g Will go for outing"
      />
      <button type="submit" class="btn btn-success text-light mt-4 mb-4">
        Add Todo
      </button>
    </form>

    <!-- table ----------------------------------------------------------------------- -->
    <table class="table w-50 m-auto table-light">
      <thead>
        <tr>
          <th scope="col">To do</th>
          <th scope="col">Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="(item, index) of toDoArray"
          v-bind:key="item.id"
          class="listitem"
        >
          <th scope="row">
            <input
              class="form-control form-control-lg w-100 m-auto bg-light"
              :class="{ done: item.done }"
              v-on:click="toggle(item)"
              v-model="item.content"
            />
          </th>
          <td>
            <!-- <button @click="updateText( item.content,index)"  class="btn btn-success px-2 mx-1 " >Update</button> -->
            <button @click="deleteToDo(index)" class="btn btn-danger mx-1">
              x
            </button>
          </td>
        </tr>
      </tbody>
    </table>

    <!-- ------------------------------------------------------------------------------------------>
    <button
      @click="markAllDone"
      class="btn btn-primary text-light mt-4 m-4 px-4"
    >
      All DONE
    </button>
    <button @click="removeAll" class="btn btn-danger">REMOVE ALL</button>
    <!-- ------------------------------------------------------------------------------------------>
  </div>
</template>

<script>
import { onMounted, ref, watch } from "vue";

export default {
  setup() {
    const userName = ref("SHAN");
    const toDoInput = ref("");
    const toDoArray = ref([]);

    function addNewTodo() {
      if (toDoInput.value.trim() === "") {
        return;
      }
      toDoArray.value.push({
        id: Date().now,
        done: false,
        content: toDoInput.value,
      });
      toDoInput.value = null;
    }

    function toggle(item) {
      item.done = !item.done;
    }

    function deleteToDo(index) {
      toDoArray.value.splice(index, 1);
    }

    function markAllDone() {
      toDoArray.value.forEach((item) => (item.done = true));
    }
    function removeAll() {
      toDoArray.value = [];
    }

    // function updateText(item,index){
    //   console.log(`the element at ${index} is: ${item}`)
    //   // console.log(toDoArray.value[index].content=toDoInput.value)
    //   // toDoInput.value = null;
    // }

    watch(
      toDoArray,
      (newVal) => {
        localStorage.setItem("toDoArray", JSON.stringify(newVal));
      },
      { deep: true }
    );
    watch(
      userName,
      (newVal) => {
        localStorage.setItem("userName", newVal);
      },
      { deep: true }
    );
    onMounted(() => {
      toDoArray.value = JSON.parse(localStorage.getItem("toDoArray")) || [];
      userName.value = localStorage.getItem("userName") || "";
    });

    return {
      userName,
      toDoInput,
      toDoArray,
      addNewTodo,
      toggle,
      deleteToDo,
      markAllDone,
      removeAll,
      // updateText,
    };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.done {
  text-decoration: line-through;
}
.listitem {
  cursor: pointer;
}
input {
  border: hidden;
  background-color: #ffffff;
  font-size: 22px;
}
h3 {
  padding-left: 10%;
}
</style>
