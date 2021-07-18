<template>
  <b-card>
    <b-card-header
      >Cau hoi {{ index + 1 }}: {{ questionData.content }}</b-card-header
    >
    <b-card-text>
      <b-form-group label="" v-slot="{ ariaDescribedby }">
        <b-form-checkbox-group
          v-model="selected"
          :options="questionData.options"
          :aria-describedby="ariaDescribedby"
          name="flavour-2a"
          @change="check"
          stacked
        ></b-form-checkbox-group>
      </b-form-group>
    </b-card-text>
  </b-card>
</template>
<script>
export default {
  props: {
    question: {
      type: Object,
      default: () => {},
    },
    index: {
      type: Number,
      default: () => 0,
    },
  },
  data() {
    return {
      selected: [],
      questionData: {},
    };
  },
  watch: {
    question(val) {
      if (val && val.selectedValue) {
        this.selected.push(val.selectedValue);
      }
      this.questionData = val;
    },
  },
  methods: {
    isDisabled(fruit) {
      const { selectedFruit } = this;
      return selectedFruit.length === 1 && selectedFruit[0] === fruit.name;
    },
    check() {
      const item = this.selected[this.selected.length - 1];
      this.selected = [];
      this.selected.push(item);
      this.$emit("updateSelect", this.questionData.id, item);
    },
  },
};
</script>
