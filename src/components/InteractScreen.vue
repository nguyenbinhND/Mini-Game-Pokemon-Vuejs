<template>
  <div class="screen">
    <div
      class="screen__inner"
      :style="{
        width: `${
          ((((920 - 16 * 4) / Math.sqrt(cardContext.length) - 16) * 3) / 4 +
            16) *
          Math.sqrt(cardContext.length)
        }px`,
      }"
    >
      <cardFlip
        v-for="(card, index) in cardContext"
        :key="index"
        :imgBackFace="`image/${card}.png`"
        :card="{ index, value: card }"
        @onFlip="checkRule"
        :isFlipped="false"
        :ref="`card-${index}`"
        :cardContext="cardContext"
      >
      </cardFlip>
    </div>
  </div>
</template>

<script>
import cardFlip from "./Card.vue";
export default {
  props: {
    cardContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  components: {
    cardFlip,
  },
  data() {
    return {
      rules: [],
    };
  },
  methods: {
    checkRule(card) {
      if (this.rules.length === 2) return false;
      this.rules.push(card);
      // console.log(this.rule);
      if (
        this.rules.length === 2 &&
        this.rules[0].value === this.rules[1].value
      ) {
        this.$refs[`card-${this.rules[0].index}`][0].onEnableDisable();
        this.$refs[`card-${this.rules[1].index}`][0].onEnableDisable();
        this.rules = [];

        const disableElement = document.querySelectorAll(
          ".screen .card.disable"
        );

        if (
          disableElement &&
          disableElement.length === this.cardContext.length - 2
        ) {
          setTimeout(() => {
            this.$emit("onResult");
          }, 500);
        }
      } else if (
        this.rules.length === 2 &&
        this.rules[0].value !== this.rules[1].value
      ) {
        setTimeout(() => {
          this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard();
          this.$refs[`card-${this.rules[1].index}`][0].onFlipBackCard();
          this.rules = [];
        }, 800);
      } else return false;
    },
  },
};
</script>

<style>
.screen {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  background-color: var(--dark);
  color: var(--light);
}

.screen__inner {
  width: calc(424px);
  display: flex;
  flex-wrap: wrap;
  margin: 2rem auto;
}
</style>
