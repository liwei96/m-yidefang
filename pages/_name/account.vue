<template>
  <div id="account">
    <div class="container-fluid m">
      <top-view :jkl="jkl"></top-view>
      <p>关注公众号了解更多新房资讯</p>
      <img src="~/assets/serve.jpg" alt class="two" />
      <button @click="down">保存二维码图片</button>
      <div class="line"></div>
    </div>
    <div class="di">
      <img src="~/assets/server.jpg" alt />
    </div>
  </div>
</template>
<script>
import '@/static/css/foot.css'
import topView from "@/components/header.vue";
export default {
  async asyncData(context) {
    let token = context.store.state.cookie.token;
    let jkl = context.params.name;
    let other = context.store.state.cookie.other;
    let city = context.store.state.cookie.city;
    let [res] = await Promise.all([
      context.$axios
        .get("/jy/phone/head/foot", {
          params: {
            city: city,
            token: token,
            other: other,
          },
        })
        .then((resp) => {
          let data = resp.data;
          // console.log(data)
          return data;
        }),
    ]);
    return {
      jkl: jkl,
      tel:res.common.phone,
      title:res.common.header.title,
      description:res.common.header.description,
      keywords:res.common.header.keywords
    };
  },
  head() {
    return {
      title: this.title || "易得房-关注公众号",
      meta: [
        {
          name: "description",
          content: this.description ||
            "易得房"
        },
        {
          name: "Keywords",
          content: this.keywords || "易得房"
        }
      ]
    };
  },
  data() {
    return {
      imgs: "http://testmobile.edefang.net/_nuxt/img/serve.c07e957.jpg",
      jkl: "",
    };
  },
  components: {
    "top-view": topView,
  },
  methods: {
    goback() {
      this.$router.go(-1);
    },
    down() {
      var alink = document.createElement("a");
      alink.href = this.imgs;
      alink.download = "pic"; //图片名
      alink.click();
    },
  },
};
</script>
<style scoped>


p {
  color: #D1A23D;
  font-size: 0.875rem;
  text-align: center;
  margin-top: 3.875rem;
  margin-bottom: 1.625rem;
}
.two {
  width: 44%;
  margin-left: 28%;
  margin-bottom: 1.625rem;
}

button {
  width: 12.5rem;
  height: 2.25rem;
  background: linear-gradient(270deg, #D1A23D, #EBBF5F);
  border-radius: 0.25rem;
  text-align: center;
  line-height: 2.25rem;
  color: #fff;
  border: 0;
  margin-left: 23.33%;
  margin-bottom: 40px;
}
.di {
  width: 100%;
  height: auto;
}
.di img {
  width: 100%;
  height: auto;
}
</style>