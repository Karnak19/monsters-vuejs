<template>
  <b-col :size="resized" :offset="offseted">
    <div class="card" :class="{'hovered':grayscale}">
      <div class="card-header justify-center" :style="{padding:'5px'}">
        <font-awesome-icon
          v-for="(el, i) in stars"
          :key="i"
          :icon="star"
          size="lg"
          :style="{color:'red'}"
        />
      </div>
      <div class="card-image" @click="click">
        <img :src="picture" alt />
      </div>
      <div class="card-body permanent">{{name}}</div>
      <footer class="card-footer">
        <p
          class="card-footer-item has-background-danger permanent has-text-white is-size-3"
        >{{attack}}</p>
        <p
          class="card-footer-item has-background-warning permanent has-text-dark is-size-3"
        >{{defense}}</p>
      </footer>
    </div>
  </b-col>
</template>

<script>
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
import { faSpinner, faStar } from "@fortawesome/free-solid-svg-icons";

import Column from "@/components/Column.vue";

export default {
  name: "Monster",
  components: {
    FontAwesomeIcon,
    "b-col": Column
  },
  data() {
    return {
      star: faStar,
      stars: []
    };
  },
  computed: {
    resized() {
      return `is-${this.size}`;
    },
    offseted() {
      return `is-offset-${this.offset}`;
    }
  },
  created() {
    for (let i = 0; i < parseInt(this.level, 10); i++) {
      this.stars.push(" ");
    }
  },
  props: {
    id: Number,
    attack: Number,
    defense: Number,
    name: String,
    picture: String,
    description: String,
    special: String,
    level: String,
    click: Function,
    size: Number,
    offset: Number,
    grayscale: Boolean
  }
};
</script>

<style scoped>
.justify-center {
  justify-content: center;
}

.hovered {
  filter: grayscale(50%);
  cursor: pointer;
  opacity: 0.7;
  transition: opacity 0.5s;
}

.hovered:hover {
  filter: none;
  opacity: 1;
  transition: opacity 0.5s;
}

.permanent {
  font-family: "Permanent Marker", cursive;
}
</style>