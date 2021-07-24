<template>
  <b-container class="mt-4">
    <div>
      <b-row>
        <b-col><h1>Chọn đề thi</h1></b-col>
        <b-col><b-button class="float-right mt-2" @click="goHistory">Lịch sử làm bài</b-button></b-col>
      </b-row>
      <div class="my-2">
        <b-form-input
          v-model="keyword"
          placeholder="Tìm theo tên"
          @change="find"
        ></b-form-input>
      </div>
    </div>
    <b-list-group>
      <b-list-group-item v-for="(item, index) in tests" :key="index">
        <p v-b-toggle="`collapse-${index}`" class="m-1">{{ item.name }}</p>
        <!-- Element to collapse -->
        <b-collapse :id="`collapse-${index}`">
          <b-card :title="item.name" tag="article">
            <b-card-text>
              {{ item.infomation }}
            </b-card-text>
            <b-card-text>
              Số câu hỏi: {{ item.questions.length }} câu.
            </b-card-text>
            <b-card-text> Thời gian {{ item.time }} phút. </b-card-text>
            <b-button href="#" variant="primary" @click="playTest(item.id)"
              >Go</b-button
            >
          </b-card>
        </b-collapse>
      </b-list-group-item>
    </b-list-group>
  </b-container>
</template>
<script>
import dataTest from "~/tests.json";
export default {
  data() {
    return {
      tests: dataTest,
      keyword: "",
    };
  },
  mounted() {
    const listTest = JSON.parse(window.localStorage.getItem("list-test"));
    console.log(listTest);
  },
  methods: {
    playTest(id) {
      this.$router.push(`/test/${id}`);
    },
    find() {
      this.tests = dataTest.filter((item) => {
        // return this.toSlug(item.name.toLowerCase()).includes(
        //   this.toSlug(this.keyword.toLowerCase())
        // );
        return item.name.toLowerCase().includes(this.keyword.toLowerCase());
      });
    },
    toSlug(str) {
      // Chuyển hết sang chữ thường
      str = str.toLowerCase();

      // xóa dấu
      str = str
        .normalize("NFD") // chuyển chuỗi sang unicode tổ hợp
        .replace(/[\u0300-\u036f]/g, ""); // xóa các ký tự dấu sau khi tách tổ hợp

      // Thay ký tự đĐ
      str = str.replace(/[đĐ]/g, "d");

      // Xóa ký tự đặc biệt
      str = str.replace(/([^0-9a-z-\s])/g, "");

      // Xóa khoảng trắng thay bằng ký tự -
      str = str.replace(/(\s+)/g, "-");

      // Xóa ký tự - liên tiếp
      str = str.replace(/-+/g, "-");

      // xóa phần dư - ở đầu & cuối
      str = str.replace(/^-+|-+$/g, "");

      // return
      return str;
    },
    goHistory(){
      this.$router.push('/test-history')
    }
  },
};
</script>
