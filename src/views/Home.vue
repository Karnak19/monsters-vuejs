<template>
  <div class="home">
    <WithMonsters />
  </div>
</template>

<script>
import WithMonsters from "@/components/WithMonsters.vue";

export default {
  name: "home",
  components: {
    WithMonsters
  }
};
</script>

<style scoped>
.home {
  margin-top: 15px;
}
</style>
