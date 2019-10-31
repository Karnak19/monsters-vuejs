<template>
  <div class="columns">
    <b-loading :is-full-page="true" :active.sync="isLoading"></b-loading>
    <div class="column columns is-3 is-multiline content-start">
      <!-- SELECTED TEAM -->
      <div class="column">
        <h1 class="title">Slots: {{selectedCount}}</h1>
        <b-field label="Name">
          <b-input v-model="name"></b-input>
        </b-field>
        <b-button type="is-primary" @click="sendTeam" :disabled="selectedCount > 0">Post your team !</b-button>
      </div>
      <Monster
        :size="8"
        :offset="2"
        v-for="selectedMonster in selected"
        :key="selectedMonster.id"
        v-bind="selectedMonster"
        :click="() => deleteMonster(selectedMonster)"
      />
    </div>
    <div class="columns column is-9 is-multiline content-start">
      <!-- ALL MONSTERS AVAILABLE -->
      <Monster
        grayscale
        :size="2"
        v-for="monster in monsters"
        :key="monster.id"
        v-bind="monster"
        :click="() => selectMonster(monster)"
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
      name: "",
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
    selectMonster: function(monster) {
      if (this.selectedCount > 0) {
        this.selected.push(monster);
        if (this.selectedCount - monster.level < 0) {
          this.$buefy.toast.open({
            duration: 2000,
            position: "is-bottom",
            type: "is-danger",
            message: "This monster is too powerful !"
          });
        } else {
          this.selectedCount = this.selectedCount - monster.level;
          this.monsters = this.monsters.filter(mnst => mnst.id !== monster.id);
        }
      } else {
        this.$buefy.toast.open({
          duration: 2000,
          position: "is-bottom",
          type: "is-danger",
          message: "You already have enough monsters !"
        });
      }
    },
    deleteMonster: function(monsterToDelete) {
      this.selected = this.selected.filter(monster => {
        return monster.id != monsterToDelete.id;
      });
      this.selectedCount =
        this.selectedCount + parseInt(monsterToDelete.level, 10);
      this.monsters = [monsterToDelete, ...this.monsters];
    },
    sendTeam: function() {
      if (this.selectedCount < 0) {
        this.$buefy.toast.open({
          duration: 2000,
          position: "is-top",
          type: "is-warning",
          message: "Whoops, you can't post that team !"
        });
      } else {
        this.isLoading = true;
        axios
          .post("http://192.168.172.109:5000/player/insert", {
            name: this.name,
            team: this.selected
          })
          .then(res => {
            console.log(res);
            localStorage.setItem("name", this.name);
          })
          .catch(err => {
            this.isError = true;
            this.error = err.message;
          })
          .finally(() => {
            this.isLoading = false;
            this.$buefy.toast.open({
              duration: 2000,
              position: "is-top",
              type: "is-success",
              message: "Your team was successfully sent !"
            });
          });
      }
    }
  },
  mounted() {
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