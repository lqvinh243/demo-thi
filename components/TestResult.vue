<template>
  <b-card title="Kết quả" class="mb-2">
    <b-row>
      <b-col>
        <p>
          Số câu đúng :<span style="font-weight: bold">
            {{ result && result.numberOfCorrect }}</span
          >
        </p>
        <p>
          Số câu sai :<span style="font-weight: bold">
            {{ result && result.numberOfWrong }}</span
          >
        </p>
        <p>
          Số câu chưa làm :
          <span style="font-weight: bold">{{
            result && result.numberOfNotCompl
          }}</span>
        </p>
        <p>
          Xếp loại:
          <span style="font-weight: bold">
            {{ result && result.score >= 0.5 ? "Đạt" : "Chưa đạt" }}</span
          >
        </p>
      </b-col>
      <b-col style="text-align: center">
        <p>Điểm số</p>
        <p style="font-size: 40px; font-weight: bol">
          {{ result && result.score * 10 }}
        </p>
      </b-col>
    </b-row>
  </b-card>
</template>
<script>
export default {
  props: {
    result: {
      type: Object,
      default: () => {},
    },
  },
};
</script>