<template>
  <div class="about">
    <div class="columns is-multiline">
      <div class="column is-10 is-offset-1">
        <b-button type="is-primary" expanded @click="fight">Fight !</b-button>
      </div>
      <div class="columns is-multiline column is-5 content-start">
        <div class="column is-12">
          <h1>{{team.name}}</h1>
        </div>
        <Monster :size="4" v-for="monster in team.monsters" :key="monster.id" v-bind="monster" />
      </div>
      <div class="columns is-multiline column is-5 is-offset-2 content-start">
        <div class="column is-12">
          <h1>{{opponentTeam.name}}</h1>
        </div>
        <Monster
          :size="4"
          v-for="monster in opponentTeam.monsters"
          :key="monster.id"
          v-bind="monster"
        />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

import Monster from "@/components/Monster.vue";
export default {
  name: "About",
  components: {
    Monster
  },
  data() {
    return {
      name: "",
      team: {
        name: "",
        monsters: []
      },
      opponentTeam: {
        name: "",
        monsters: []
      },
      fighters: []
    };
  },
  methods: {
    fight: function() {
      while (true) {
        this.fighters = [this.team.monsters[0], this.opponentTeam.monsters[0]];
        const fighter0 = this.fighters[0];
        const fighter1 = this.fighters[1];

        if (fighter0.attack > fighter1.defense) {
          this.opponentTeam.monsters = this.opponentTeam.monsters.filter(
            monster => monster.id !== fighter1.id
          );
          this.$buefy.toast.open({
            message: "Boom !",
            queue: false,
            position: "is-top",
            type: "is-warning"
          });
        } else if (fighter0.attack === fighter1.defense) {
          this.team.monsters = this.team.monsters.filter(
            monster => monster.id !== fighter0.id
          );
          this.opponentTeam.monsters = this.opponentTeam.monsters.filter(
            monster => monster.id !== fighter1.id
          );
          this.$buefy.toast.open({
            message: "They trade !",
            queue: false,
            position: "is-top",
            type: "is-danger"
          });
        } else if (fighter0.attack < fighter1.defense) {
          this.team.monsters = this.team.monsters.filter(
            monster => monster.id !== fighter0.id
          );
          this.opponentTeam.monsters = this.opponentTeam.monsters.map(el => {
            if (el.id === fighter1.id) {
              return { ...el, defense: fighter1.defense - fighter0.attack };
            } else {
              return el;
            }
          });
        } else {
          const fighterToCrop = this.opponentTeam.monsters.find(
            el => el.id === fighter1.id
          );
          console.log(fighterToCrop);
        }
        this.fighters = [];
        if (
          this.team.monsters.length === 0 ||
          this.opponentTeam.monsters === 0
        ) {
          if (
            this.team.monsters.length === 0 &&
            this.opponentTeam.monsters === 0
          ) {
            this.$buefy.toast.open({
              message: `THEY BOTH LOOSE !`,
              queue: false,
              position: "is-top",
              type: "is-success"
            });
            return false;
          } else {
            if (this.team.monsters.length === 0) {
              this.$buefy.toast.open({
                message: `${this.opponentTeam.name} WIN !`,
                queue: false,
                position: "is-top",
                type: "is-success"
              });
              return false;
            }
            if (this.opponentTeam.monsters.length === 0) {
              this.$buefy.toast.open({
                message: `${this.team.name} WIN !`,
                queue: false,
                position: "is-top",
                type: "is-success"
              });
              return false;
            }
          }
        }
      }
    }
  },
  async mounted() {
    this.name = localStorage.getItem("name");
    const [team, opponent] = await Promise.all([
      axios.get(`http://192.168.172.109:5000/player/${this.name}`),
      axios.get(`http://192.168.172.109:5000/player/random/${this.name}`)
    ]);
    this.team = { name: team.data.name, monsters: team.data.team };
    this.opponentTeam = {
      name: opponent.data.name,
      monsters: opponent.data.team
    };
  }
};
</script>

<style scoped>
.content-start {
  align-content: flex-start;
}
</style>