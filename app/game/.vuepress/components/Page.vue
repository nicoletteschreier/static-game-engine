<template>
  <div class="prompt">
    <span class="font-bold" v-if="show">{{ instructions }}</span>
    <div class="page-nav" v-if="show">
      <router-link :to="link || './'">{{ action }}</router-link>
    </div>
  </div>
</template>
<script>
import { emitter } from "@theme/utils/emitter";

export default {
  props: ["url", "action", "condition", "instructions"],

  data() {
    return {
      show: false,
      link: this.url,
    };
  },
  methods: {
    getInventory(id) {
      if (this.condition == id || this.condition == "none") {
        this.show = true;
      } else {
        this.show = false;
      }
    },
    goToRoom(url) {
      this.$router.replace({ path: url });
    },
  },
  created() {
    //initially, set home page to show followup
    this.getInventory(1);

    emitter.on("item_added", (id) => {
      console.log(id);
      this.getInventory(id);
    });

    emitter.on("showResult", (id) => {
      console.log("show results");
      console.log(id);
      this.getInventory(id);
    });
  },
  mounted() {
    emitter.on("*", (type, e) => console.log("listening to page ", type, e));
  },
};
</script>
<style lang="stylus">
.prompt {
  padding-top: 10px;
  font-size: 28px;
}

.page-nav {
  font-weight: bold;
  padding-top: 10px;
  text-decoration: underline;
  display: block;
  cursor: pointer;
}
</style>
