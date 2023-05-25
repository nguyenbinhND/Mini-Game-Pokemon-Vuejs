<template>
  <MainScreenVue
    v-if="status === 'default'"
    @onStart="handelBefor($event)"
  ></MainScreenVue>
  <InteractScreen
    v-if="status === 'match'"
    :cardContext="setting.cardContext"
    @onResult="onResult"
  ></InteractScreen>
  <GetResult
    v-if="status === 'result'"
    :timer="timer"
    @onStartAgain="StartAgain"
  ></GetResult>
</template>

<script>
import MainScreenVue from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import GetResult from "./components/GetResult.vue";

import { shuffled } from "./utils/array";

export default {
  name: "App",
  data() {
    return {
      status: "default",
      timer: 0,

      setting: {
        totalOfBlock: 0,
        cardContext: [],
        startAt: null,
      },
    };
  },
  components: {
    MainScreenVue,
    InteractScreen,
    GetResult,
  },
  methods: {
    handelBefor(config) {
      console.log("heandel running befor", config);
      this.setting.totalOfBlock = config.totalBlock;
      const firstCard = Array.from(
        { length: this.setting.totalOfBlock / 2 },
        (_, i) => i + 1
      );
      const secondCard = [...firstCard];
      const card = [...firstCard, ...secondCard];
      this.setting.cardContext = shuffled(card);
      // console.log(this.setting.cardContext);
      this.setting.startAt = new Date().getTime();
      this.status = "match";
    },
    onResult() {
      this.timer = new Date().getTime() - this.setting.startAt;
      this.status = "result";
    },
    StartAgain() {
      this.status = "default";
    },
  },
};
</script>

<style>
#app {
}
</style>
