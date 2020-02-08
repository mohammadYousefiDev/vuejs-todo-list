<template>
  <div class="home">
    <todo/>
  </div>
</template>

<script>
// @ is an alias to /src
import todo from "@/components/todo.vue";

export default {
  name: "Home",
  components: {
    todo
  }
};
</script>
