<template>
  <div class="row">
    <h2 v-show="listInfo.isFirst">欢迎使用</h2>
    <h2 v-show="listInfo.isLoading">加载中,请稍等</h2>
    <h2 v-show="listInfo.errMsg">{{ listInfo.errMsg }}</h2>
    <div
      v-show="listInfo.users.length"
      class="card"
      v-for="userObj in listInfo.users"
      :key="userObj.login"
    >
      <a :href="userObj.html_url" target="_blank">
        <img src="userObj.avatar_url" style="width: 100px" />
      </a>
      <p class="card-text">{{ userObj.login }}</p>
    </div>
  </div>
</template>

<script>
import PubSub from "pubsub-js";
export default {
  name: "List",
  data() {
    return {
      listInfo: { users: [], isFirst: true, isLoading: false, errMsg: "" },
    };
  },
  methods: {
    saveData(_, data) {
      // console.log("List收到了用户信息:", data);
      this.listInfo = { ...this.listInfo, ...data };
    },
  },
  mounted() {
    //组件一挂载就绑定事件
    // this.$bus.$on("get-list-data", this.saveData);

    //组件一挂载就订阅消息
    PubSub.subscribe("get-list-data", this.saveData);
  },

  beforeDestroy() {
    //组件销毁前取消绑定事件
    // this.$bus.$off('get-list-data')

    // 组件销毁前取消订阅
    PubSub.unsubscribe(this.token);
  },
};
</script>

<style scoped>
.album {
  min-height: 50rem; /* Can be removed; just added for demo purposes */
  padding-top: 3rem;
  padding-bottom: 3rem;
  background-color: #f7f7f7;
}

.card {
  float: left;
  width: 33.333%;
  padding: 0.75rem;
  margin-bottom: 2rem;
  border: 1px solid #efefef;
  text-align: center;
}

.card > img {
  margin-bottom: 0.75rem;
  border-radius: 100px;
}

.card-text {
  font-size: 85%;
}
</style>