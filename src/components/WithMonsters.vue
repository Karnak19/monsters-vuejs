<template>
  <div class="columns">
    <div class="column columns is-3 is-multiline content-start">
      <h1 class="title">Slots: {{selectedCount}}</h1>
      <b-button @click="isModalOpened = true">Modal</b-button>
      <Monster
        :size="8"
        :offset="2"
        v-for="selectedMonster in selected"
        :key="selectedMonster.id"
        v-bind="selectedMonster"
        :click="() => deleteMonster(selectedMonster)"
      />
    </div>
    <b-loading :is-full-page="true" :active.sync="isLoading"></b-loading>
    <div class="columns column is-9 is-multiline content-start">
      <Monster
        :size="2"
        v-for="monster in monsters"
        :key="monster.id"
        v-bind="monster"
        :click="() => select(monster)"
      />
    </div>
    <b-modal :active-sync="isModalOpened">
      <div class="card">
        <div class="card-content">toto</div>
      </div>
    </b-modal>
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
      isModalOpened: false,
      isError: false,
      error: "",
      monsters: [],
      selected: [],
      selectedCount: 20
    };
  },
  methods: {
    select: function(monster) {
      if (this.selectedCount > 0) {
        this.selected.push(monster);
        if (this.selectedCount - monster.level < 0) {
          alert("This monster is too powerful !");
        } else {
          this.selectedCount = this.selectedCount - monster.level;
          this.monsters = this.monsters.filter(mnst => mnst.id !== monster.id);
        }
      } else {
        alert("STOP");
      }
    },
    deleteMonster: function(monsterToDelete) {
      this.selected = this.selected.filter(monster => {
        console.log(monsterToDelete.id);
        return monster.id != monsterToDelete.id;
      });
      this.selectedCount =
        this.selectedCount + parseInt(monsterToDelete.level, 10);
      this.monsters = [monsterToDelete, ...this.monsters];
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