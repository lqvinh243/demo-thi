<template>
  <div class="container mt-4">
    <template> </template>
    <template>
      <b-row>
        <b-col cols="4">
          <TestInfo :testInfo="testInfo" :isStart="isStart" />
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
                  Hết thời gian làm bài hệ thống sẽ tự thu bài.Bạn có thể nộp
                  bài trước khi thời gian kết thúc bằng cách nhấn nút
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
import dataTest from "~/tests.json";
export default {
  components: {
    ContestTest,
    StartTest,
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
      testInfo: {
        durationMinute: 15,
        durationSecond: 0,
        totalQuestion: 0,
        title: "",
      },
      dataSample: dataTest,
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
    this.dataSample = JSON.parse(JSON.stringify(this.dataSample));
    this.dataTest = {
      ...this.dataSample.find(
        (item) => item.id === parseFloat(this.$route.params.id)
      ),
    };
    this.dataTest.questions = this.shuffle(this.dataTest.questions);
    console.log(
      this.dataTest.questions.filter((item) => item.selectedValue !== "")
    );
    this.testInfo.durationMinute = this.dataTest.time;
    this.testInfo.title = this.dataTest.name;
    this.testInfo.totalQuestion = this.dataTest.questions.length;
    this.selectdQuestion = this.dataTest.questions[indexDefault];
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
    start() {
      if (this.testInfo.durationSecond === 0) {
        this.testInfo.durationMinute -= 1;
        this.testInfo.durationSecond = 59;
      } else this.testInfo.durationSecond--;

      if (this.testInfo.durationSecond === 0) {
        clearInterval(this.timer);
        alert("Hết giờ làm bài");
        this.finishTest();
      }
    },
    startTest() {
      this.isStart = true;
      this.timer = setInterval(() => this.start(), 1000);
    },
    finishTest() {
      clearInterval(this.timer);
      const data = JSON.stringify(this.dataTest);
      window.localStorage.setItem("test", data);
      const listTest = JSON.parse(window.localStorage.getItem("list-test"));
      if (!listTest) {
        const newList = [];
        const testTm = { ...this.dataTest };
        testTm["date"] = new Date().getTime();
        newList.push(testTm);
        window.localStorage.setItem("list-test", JSON.stringify(newList));
      } else {
        const testTm = { ...this.dataTest };
        testTm["date"] = new Date().getTime();
        listTest.push(testTm);
        window.localStorage.setItem("list-test", JSON.stringify(listTest));
      }

      this.$router.push("/test-result");
    },
    shuffle(array) {
      var currentIndex = array.length,
        randomIndex;

      // While there remain elements to shuffle...
      while (0 !== currentIndex) {
        // Pick a remaining element...
        randomIndex = Math.floor(Math.random() * currentIndex);
        currentIndex--;

        // And swap it with the current element.
        [array[currentIndex], array[randomIndex]] = [
          array[randomIndex],
          array[currentIndex],
        ];
      }

      return array;
    },
  },
};
</script>
<style scoped>
* >>> ol {
  text-align: left;
}
</style>
