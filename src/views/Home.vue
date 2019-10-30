<template>
  <div class="home">
    <WithMonsters />
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from "@/components/HelloWorld.vue";
import WithMonsters from "@/components/WithMonsters.vue";

export default {
  name: "home",
  components: {
    HelloWorld,
    WithMonsters
  }
};
</script>

<style scoped>
.home {
  margin-top: 15px;
}
</style>
