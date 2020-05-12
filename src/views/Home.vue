<template>
  <div class="home">
    
    <img alt="Vue logo" src="../assets/logo.png" />
     <TodoList/>
    <!-- <HelloWorld msg="Welcome to Your Vue.js App" /> -->
   
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from "@/components/HelloWorld.vue";
import TodoList from "../components/Todolist";



export default {
  name: "Home",
  components: {
    // HelloWorld,
    TodoList
  }
};
</script>
