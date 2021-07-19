<template>
  <b-card title="Ket qua" class="mb-2">
    <b-row>
      <b-col>
        <p>So cau dung : {{ result && result.numberOfCorrect }}</p>
        <p>So cau sai : {{ result && result.numberOfWrong }}</p>
        <p>So cau chua lam : {{ result && result.numberOfNotCompl }}</p>
        <p>
          Xep loai: {{ result && result.score >= 0.5 ? "Dat" : "Chua dat" }}
        </p>
      </b-col>
      <b-col style="text-align: center">
        <p>Diem</p>
        <p>{{ result && result.score * 10 }}</p>
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