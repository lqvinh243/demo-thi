<template>
  <b-card
    title="Đề thi tiếng Anh"
    align="center"
    tag="article"
    style="max-width: 20rem"
    class="mb-2"
  >
    <b-card-text> Người đăng </b-card-text>
    <b-card-text> Số câu hỏi : {{ totalQuestion }} </b-card-text>
    <b-card-text>
      Thời gian thực hiện : {{ durationMinute }}:{{ mapDurationSecond }}
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
  },
  data() {
    return {
      durationMinute: 15,
      durationSecond: 0,
      totalQuestion: 0,
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
      },
      deep: true,
    },
  },
  mounted() {
    this.durationMinute = this.testInfo.durationMinute;
    this.durationSecond = this.testInfo.durationSecond;
    this.totalQuestion = this.testInfo.totalQuestion;
  },
};
</script>
