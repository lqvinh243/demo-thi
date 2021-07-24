<template>
  <b-container class="mt-4">
    <div>
      <b-row>
        <b-col><h1>Lịch sử làm bài</h1></b-col>
        <b-col
          ><b-button class="float-right mt-2" @click="goHome"
            >Quay lại chọn đề</b-button
          ></b-col
        >
      </b-row>
    </div>
    <div v-if="listTest && listTest.length">
      <b-button variant="danger" class="my-1" @click="clearHistory()"
        >Xóa lịch sử làm bài</b-button
      >

      <b-list-group>
        <b-list-group-item v-for="(item, index) in listTest" :key="index">
          <div v-b-toggle="`collapse-${index}`" class="m-1">
            <p>
              {{ `${item.name} ` }}
            </p>
            <p>
              {{ `Thời gian làm: ${convertTime(item.date)}` }}
            </p>
          </div>

          <!-- Element to collapse -->
          <b-collapse :id="`collapse-${index}`">
            <b-card :title="`${item.name} `" tag="article">
              <b-card-text>
                {{ item.infomation }}
              </b-card-text>
              <b-card-text>
                Số câu hỏi: {{ item.questions.length }} câu.
              </b-card-text>
              <b-card-text> Thời gian {{ item.time }} phút. </b-card-text>
              <b-card-text> Điểm số {{ calScore(item) }} điểm. </b-card-text>
              <b-button
                href="#"
                variant="primary"
                @click="goDetailHistory(item.date)"
                >Xem lại</b-button
              >
            </b-card>
          </b-collapse>
        </b-list-group-item>
      </b-list-group>
    </div>
    <p v-else>Không có lịch sử làm bài</p>
  </b-container>
</template>
<script>
export default {
  data() {
    return {
      keyword: "",
      listTest: [],
    };
  },
  mounted() {
    const listTest = JSON.parse(window.localStorage.getItem("list-test"));
    this.listTest = listTest;
    console.log(this.listTest);
  },
  methods: {
    playTest(id) {
      this.$router.push(`/test/${id}`);
    },
    convertTime(time) {
      var date = new Date(time); // create Date object
      const timeString = `${date.getHours()}:${date.getMinutes()} ${date.getDay()}/${
        date.getMonth() + 1
      }/${date.getFullYear()}`;
      return timeString; // result: Wed Jan 12 2011 12:42:46 GMT-0800 (PST)
    },
    calScore(data) {
      const numberOfCorrect = data.questions.filter(
        (item) =>
          item.selectedValue ===
          item.options.find((ii) => ii.isCorrect === true).value
      ).length;

      return (numberOfCorrect / data.questions.length) * 10;
    },
    goHome() {
      this.$router.push("/");
    },
    goDetailHistory(date) {
      this.$router.push(`/test-result/${date}`);
    },
    clearHistory() {
      localStorage.removeItem("list-test");
      alert("Xóa lịch sử bài thi thành công!");
      this.$router.push("/");
    },
  },
};
</script>
