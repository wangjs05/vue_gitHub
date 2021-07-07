<template>
  <section class="jumbotron">
    <h3 class="jumbotron-heading">Search Github Users</h3>
    <div>
      <input
        @keyup.enter="search"
        v-model="keyword"
        type="text"
        placeholder="enter the name you search"
      />&nbsp;<button @click="search" @keyup.enter="search">Search</button>
    </div>
  </section>
</template>

<script>
import axios from "axios";
export default {
  name: "Search",
  data() {
    return {
      keyword: "",
    };
  },
  methods: {
    async search() {
      // 请求之前通知List组件更新他的data
      this.$bus.$emit("get-list-data", { isFirst: false, isLoading: true });
      try {
        //发送请求
        const response = await axios.get(
          "https://api.github.com/search/users",
          {
            params: { q: this.keyword },
          }
        );
        const { items } = response.data;
        //请求成功后通知List组件更新他的data
        this.$bus.$emit("get-list-data", { isLoading: false, users: items });
      } catch (error) {
        //请求失败后通知List更新他的data
        this.$bus.$emit("get-list-data", {
          isLoading: false,
          users: [],
          errMsg: error.message,
        });
      }
    },
    // search() {
    //   axios
    //     .get("https://api.github.com/search/users", {
    //       params: { q: this.keyword },
    //     })
    //     .then(
    //       (response) => {
    //         console.log("成功了", response.data.items);
    //       },
    //       (error) => {
    //         console.log("请求失败", error.message2);
    //       }
    //     );
    // },
  },
};
</script>

<style>
</style>