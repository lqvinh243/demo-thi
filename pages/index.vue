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

          <StartTest @startTest="startTest" @finishTest="finishTest" />
        </b-col>
        <b-col>
          <b-card
            title="Hướng dẫn làm bài thi"
            align="center"
            tag="article"
            class="mb-2"
            v-if="!isStart"
          >
            <b-card-text>
              <ol>
                <li>Nhấn nút bắt đầu để làm bài thi</li>
                <li>Ở mỗi câu hỏi chọn đáp án đúng</li>
                <li>
                  Hết thời gian làm bài hệ thống sẽ tự thu bài.Bạn có thể nộp bài trước khi thời gian kết thúc bằng cách nhấn nút 
                  <b>Nộp bài</b>
                </li>
              </ol>
            </b-card-text>
          </b-card>
          <template v-if="isStart">
            <div>
              <ContestTest
                :question="selectdQuestion"
                :index="indexSelect"
                @updateSelect="updateSelect"
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
import ContestTest from "~/components/ContentTest";
import StartTest from "~/components/StartTest";
import TestInfo from "~/components/TestInfo";
import data from "~/test.json";
export default {
  components: {
    ContestTest,
    StartTest,
    TestInfo,
  },
  data() {
    return {
      selected: [], // Must be an array reference!
      questions: data.questions,
      selectdQuestion: {},
      indexSelect: 0,
      timer: null,
      isStart: false,
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
  },
  mounted() {
    const indexDefault = 0;
    this.testInfo.totalQuestion = this.questions.length;
    this.selectdQuestion = this.questions[indexDefault];
    this.indexSelect = indexDefault;
  },
  methods: {
    check() {
      const item = this.selected[this.selected.length - 1];
      this.selected = [];
      this.selected.push(item);
    },
    next() {
      this.indexSelect++;
      this.selectdQuestion = this.questions[this.indexSelect];
    },
    pre() {
      this.indexSelect--;
      this.selectdQuestion = this.questions[this.indexSelect];
    },
    updateSelect(id, selectedValue) {
      const item = this.questions.find((ii) => ii.id === id);
      if (item) {
        item.selectedValue = selectedValue;
      }
    },
    start() {
      if (this.testInfo.durationSecond === 0) {
        this.testInfo.durationMinute -= 1;
        this.testInfo.durationSecond = 59;
      } else this.testInfo.durationSecond--;

      if (this.testInfo.durationMinute === 0) {
        clearInterval(this.timer);
      }
    },
    startTest() {
      this.isStart = true;
      this.timer = setInterval(() => this.start(), 1000);
    },
    finishTest() {
      const data = JSON.stringify(this.questions);
      window.localStorage.setItem("test", data);
      this.$router.push("/test-result");
    },
  },
};
</script>
<style scoped>
* >>> ol {
  text-align: left;
}
</style>
