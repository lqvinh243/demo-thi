<template>
  <div>
    <div v-if="!isTryAgain">
      <b-card
        v-if="!isStart"
        align="center"
        class="mb-2"
        style="max-width: 20rem"
      >
        <b-button href="#" variant="primary" @click="startTest()"
          >Bat dau lam bai</b-button
        >
      </b-card>
      <template v-if="isStart">
        <div>
          <b-card align="center" class="mb-2" style="max-width: 20rem">
            <b-button href="#" variant="danger" @click="finishTest()"
              >Nop bai</b-button
            >
          </b-card>
        </div>
      </template>
    </div>
    <div v-else>
      <b-card
        v-if="!isStart"
        align="center"
        class="mb-2"
        style="max-width: 20rem"
      >
        <b-button href="#" variant="primary" @click="tryAgain()"
          >Thu lai</b-button
        >
      </b-card>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    isTryAgain: {
      type: Boolean,
      default: () => false,
    },
  },
  data() {
    return {
      isStart: false,
    };
  },
  methods: {
    startTest() {
      this.isStart = true;
      this.$emit("startTest", true);
    },
    finishTest() {
      this.$emit("finishTest", true);
    },
    tryAgain() {
      this.$router.push("/");
    },
  },
};
</script>