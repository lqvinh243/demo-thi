<template>
  <div class="container mt-4">
    <template> </template>
    <template>
      <b-row>
        <b-col cols="4">
          <TestInfo :testInfo="testInfo" />
          <b-card align="center" class="mb-2" style="max-width: 20rem">
            <b-card-text> Guest </b-card-text>
          </b-card>

          <StartTest :isTryAgain="true" @tryAgain="tryAgain" />
        </b-col>
        <b-col>
          <TestResult :result="result" />
          <template>
            <div>
              <ContestTest
                :question="selectdQuestion"
                :index="indexSelect"
                @updateSelect="updateSelect"
                :isShowAns="true"
              />
              <div class="mt-4">
                <b-button
                  variant="danger"
                  :disabled="disabledPre"
                  @click="pre()"
                  >Câu trước</b-button
                >
                <b-button
                  variant="success"
                  :disabled="disabledNext"
                  @click="next()"
                  >Câu kế</b-button
                >
              </div>
            </div>
          </template>
        </b-col>
      </b-row>
    </template>
  </div>
</template>

<script>
import TestResult from "~/components/TestResult";
import ContestTest from "~/components/ContentTest";
import StartTest from "~/components/StartTest";
import TestInfo from "~/components/TestInfo";
export default {
  components: {
    ContestTest,
    StartTest,
    TestResult,
    TestInfo,
  },
  data() {
    return {
      selected: [], // Must be an array reference!
      dataTest: {
        questions: [],
      },
      selectdQuestion: {},
      indexSelect: 0,
      timer: null,
      isStart: false,
      result: {
        numberOfCorrect: 0,
        numberOfWrong: 0,
        numberOfNotCompl: 0,
        score: 0,
      },
      testInfo: {
        durationMinute: 15,
        durationSecond: 0,
        totalQuestion: 0,
      },
    };
  },
  computed: {
    disabledNext() {
      return this.indexSelect + 1 === this.testInfo.totalQuestion;
    },
    disabledPre() {
      return this.indexSelect === 0;
    },
    mapDurationSecond() {
      return this.durationSecond < 10
        ? `0${this.durationSecond}`
        : this.durationSecond;
    },
    mapDurationMinute() {
      return this.durationMinute < 10
        ? `0${this.durationMinute}`
        : this.durationMinute;
    },
  },
  mounted() {
    const date = this.$route.params.date;
    if (!date) this.$router.push("/");
    const listTest = JSON.parse(window.localStorage.getItem("list-test"));

    this.dataTest = listTest.find((item) => item.date === parseFloat(date));

    const indexDefault = 0;
    this.testInfo.totalQuestion = this.dataTest.questions.length;
    this.selectdQuestion = this.dataTest.questions[indexDefault];
    this.indexSelect = indexDefault;
    this.result.numberOfCorrect = this.dataTest.questions.filter(
      (item) =>
        item.selectedValue ===
        item.options.find((ii) => ii.isCorrect === true).value
    ).length;
    this.result.numberOfWrong =
      this.dataTest.questions.length - this.result.numberOfCorrect;
    this.result.numberOfNotCompl = this.dataTest.questions.filter(
      (item) => item.selectedValue === ""
    ).length;
    this.result.score =
      this.result.numberOfCorrect / this.testInfo.totalQuestion;
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
    },
    next() {
      this.indexSelect++;
      this.selectdQuestion = this.dataTest.questions[this.indexSelect];
    },
    pre() {
      this.indexSelect--;
      this.selectdQuestion = this.dataTest.questions[this.indexSelect];
    },
    updateSelect(id, selectedValue) {
      const item = this.dataTest.questions.find((ii) => ii.id === id);
      if (item) {
        item.selectedValue = selectedValue;
      }
    },
    tryAgain() {
      this.$router.push(`/test/${this.dataTest.id}`);
    },
  },
};
</script>
<style scoped>
* >>> ol {
  text-align: left;
}
</style>
