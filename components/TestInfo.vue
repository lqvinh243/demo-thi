<template>
  <b-card
    :title="title"
    align="center"
    tag="article"
    style="max-width: 20rem"
    class="mb-2"
  >
    <b-card-text> Người đăng : Admin </b-card-text>
    <b-card-text> Số câu hỏi : {{ totalQuestion }} </b-card-text>
    <b-card-text>
      {{ titleTime }} : {{ durationMinute }}:{{ mapDurationSecond }} s
    </b-card-text>
  </b-card>
</template>
<script>
export default {
  props: {
    testInfo: {
      type: Object,
      default: () => {},
    },
    isStart: {
      type: Boolean,
      default: () => false,
    },
  },
  data() {
    return {
      durationMinute: 15,
      durationSecond: 0,
      totalQuestion: 0,
      title: "",
      titleTime: "Thời gian thực hiện",
    };
  },
  computed: {
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
  watch: {
    testInfo: {
      handler: function (val) {
        this.durationMinute = val.durationMinute;
        this.durationSecond = val.durationSecond;
        this.totalQuestion = val.totalQuestion;
        this.title = val.title;
      },
      deep: true,
    },
    isStart: {
      handler: function (val) {
        if (val === true) {
          this.titleTime = "Thời gian còn lại";
        }
      },
    },
  },
  mounted() {
    this.durationMinute = this.testInfo.durationMinute;
    this.durationSecond = this.testInfo.durationSecond;
    this.totalQuestion = this.testInfo.totalQuestion;
    this.title = this.testInfo.title;
  },
};
</script>
