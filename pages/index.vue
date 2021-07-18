<template>
  <div class="container mt-4">
    <template> </template>
    <template>
      <b-row>
        <b-col cols="4">
          <b-card
            title="De thi tin hoc"
            align="center"
            tag="article"
            style="max-width: 20rem"
            class="mb-2"
          >
            <b-card-text> Nguoi dang : Admin </b-card-text>
            <b-card-text> So cau : 2 </b-card-text>
            <b-card-text> Thoi gian : 15p </b-card-text>
          </b-card>
          <b-card class="mb-2" style="max-width: 20rem">
            <b-card-text> Nguoi dang : Admin </b-card-text>
          </b-card>

          <b-card align="center" class="mb-2" style="max-width: 20rem">
            <b-button href="#" variant="primary">Bat dau lam bai</b-button>
          </b-card>
        </b-col>
        <b-col>
          <b-card
            title="Huong dan lam bai thi"
            align="center"
            tag="article"
            class="mb-2"
          >
            <b-card-text>
              <ol>
                <li>Nhan nut bat dau de lam bai thi</li>
                <li>O moi cau hoi chon dap an dung</li>
                <li>
                  Het thoi gian lam bai he thong se tu thu bai.Ban co the nop
                  bai truoc khi thoi gian ket thuc bang cach nhan nut
                  <b>Nop bai</b>
                </li>
              </ol>
            </b-card-text>
          </b-card>
          <ContestTest
            :question="selectdQuestion"
            :index="indexSelect"
            @updateSelect="updateSelect"
          />
          <div class="mt-4">
            <b-button variant="danger" :disabled="disabledPre" @click="pre()"
              >Truoc do</b-button
            >
            <b-button variant="success" :disabled="disabledNext" @click="next()"
              >Ke tiep</b-button
            >
          </div>
        </b-col>
      </b-row>
    </template>
  </div>
</template>

<script>
import ContestTest from "~/components/ContentTest";
export default {
  components: {
    ContestTest,
  },
  data() {
    return {
      selected: [], // Must be an array reference!
      questions: [
        {
          id: 1,
          content: "Tra loi cau hoi sau:",
          options: [
            { text: "Orange", value: "orange" },
            { text: "Apple", value: "apple" },
            { text: "Pineapple", value: "pineapple" },
            { text: "Grape", value: "grape" },
          ],
          selectedValue: "pineapple",
        },
        {
          id: 2,
          content: "Hom nay an mon gi:",
          options: [
            { text: "Com", value: "com" },
            { text: "Tao", value: "tao" },
            { text: "Chuoi", value: "chuoi" },
            { text: "Sua", value: "sua" },
          ],
          selectedValue: "",
        },
      ],
      selectdQuestion: {},
      totalQuestion: 0,
      indexSelect: 0,
    };
  },
  computed: {
    disabledNext() {
      return this.indexSelect + 1 === this.totalQuestion;
    },
    disabledPre() {
      return this.indexSelect === 0;
    },
  },
  mounted() {
    const indexDefault = 0;
    this.totalQuestion = this.questions.length;
    this.selectdQuestion = this.questions[indexDefault];
    this.indexSelect = indexDefault;
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
  },
};
</script>
<style scoped>
* >>> ol {
  text-align: left;
}
</style>
