<template>
  <div class="columns">
    <div class="column columns is-3 is-multiline content-start">
      <h1 class="title">Slots: {{selectedCount}}</h1>
      <Monster
        :size="8"
        :offset="2"
        v-for="selectedMonster in selected"
        :key="selectedMonster.id"
        v-bind="selectedMonster"
      />
    </div>
    <b-loading :is-full-page="isFullPage" :active.sync="isLoading"></b-loading>
    <div class="columns column is-9 is-multiline content-start">
      <Monster
        :size="2"
        v-for="monster in monsters"
        :key="monster.id"
        v-bind="monster"
        :click="() => select(monster)"
      />
    </div>
  </div>
</template>

<script>
import axios from "axios";

import Monster from "@/components/Monster.vue";

export default {
  name: "WithMonsters",
  components: {
    Monster
  },
  data() {
    return {
      isLoading: true,
      isError: false,
      error: "",
      monsters: [],
      selected: [],
      selectedCount: 5
    };
  },
  methods: {
    select: function(monster) {
      if (this.selectedCount > 0) {
        this.selected.push(monster);
        this.selectedCount = this.selectedCount - 1;
      } else {
        alert("STOP");
      }
    }
  },
  created() {
    axios
      .get("https://hackathon-wild-hackoween.herokuapp.com/monsters")
      .then(res => {
        this.monsters = res.data.monsters;
      })
      .catch(err => {
        this.isError = true;
        this.error = err.message;
      })
      .finally(() => {
        this.isLoading = false;
      });
  }
};
</script>

<style scoped>
.content-start {
  align-content: flex-start;
}
</style>