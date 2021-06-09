<template>
  <div>
    <Nuxt class="allbox" />
    <footer v-if="home" id="foott">
      <ul v-if="false">
        <li>
          <nuxt-link :to="'/' + jkl + '/about'">关于我们</nuxt-link>
        </li>
        <li @click="zhaopin">诚聘英才</li>
        <li><nuxt-link :to="'/' + jkl + '/join'">城市加盟</nuxt-link></li>
        <li><nuxt-link :to="'/' + jkl + '/privacy'">隐私政策</nuxt-link></li>
      </ul>
      <p>&nbsp;&nbsp;电话：4006881980</p>
      <p v-if="false">
        <img src="~/assets/logobom.png" alt="" />
        网络经营许可证：
        <a href="http://www.beian.gov.cn/portal/index.do"
          >浙ICP备18057005号-1</a
        >
      </p>
    </footer>
  </div>
</template>
<script>
export default {
  async asyncData(context) {
    let jkl = context.params.name;
    return {
      jkl: jkl,
    };
  },
  data() {
    return {
      jkl: "",
      home: true,
      url: "",
      host: 0,
    };
  },
  mounted() {
    if (localStorage.getItem("uuid")) {
      $cookies.set("uuid", localStorage.getItem("uuid"));
      this.$store.dispatch("setuuid", localStorage.getItem("uuid"));
    }
    this.host = this.$store.state.host;
    // this.ws = new ReconnectingWebSocket(
    //   "ws://39.98.227.114:9509?uuid=zhYfTSJ3yQt71602841160013"
    // );
    // this.$store.dispatch("setws", this.ws);
    // console.log(456)
    // this.ws.onopen = function(){
    //   sessionStorage.setItem('type',1)
    // }
    this.jkl = this.$route.params.name;
    this.url = this.$route.path;
    var _hmt = _hmt || [];
    (function () {
      var hm = document.createElement("script");
      hm.src = "https://hm.baidu.com/hm.js?8a2d6559d657be638285016d72a796c0";
      var s = document.getElementsByTagName("script")[0];
      s.parentNode.insertBefore(hm, s);
    })();
    resetrem();
    //切换屏幕 （横屏竖屏）
    window.addEventListener("orientationchange", resetrem);
    //resize：屏幕的大小发生改变就触发监听事件resetrem
    window.addEventListener("resize", resetrem);
    function resetrem() {
      var html = document.querySelector("html"); //获取到html元素
      var width = html.getBoundingClientRect().width; //获取屏幕的宽度
      html.style.fontSize = width / 23.5 + "px";
    }
    //     (function(){
    // var src = "https://jspassport.ssl.qhimg.com/11.0.1.js?d182b3f28525f2db83acfaaf6e696dba";
    // document.write('<script src="' + src + '" id="sozz"><\/script>');
    // })();
    (function () {
      var bp = document.createElement("script");
      var curProtocol = window.location.protocol.split(":")[0];
      if (curProtocol === "https") {
        bp.src = "https://zz.bdstatic.com/linksubmit/push.js";
      } else {
        bp.src = "http://push.zhanzhang.baidu.com/push.js";
      }
      var s = document.getElementsByTagName("script")[0];
      s.parentNode.insertBefore(bp, s);
    })();
  },
  methods: {
    zhaopin() {
      window.location.href =
        "http://recruit.jy1980.com?type=1&city=" + localStorage.getItem("city");
    },
  },
  beforeRouteUpdate(to, from, next) {
    console.log(to);
  },
};
</script>
<style lang="less">
* {
  padding: 0;
  margin: 0;
}
body,
html {
  max-width: 33.75rem;
  margin: 0 auto;
}
.allbox {
  min-height: 100vh;
}
footer {
  height: 6.25rem;
  // background-color: #e9eef5;
  background: rgba(0, 0, 0, 0);
  color: #6c88a6;
  font-size: 0.75rem;
  margin-bottom: 50px;
  z-index: 1000;
  position: relative;
  ul {
    display: flex;
    justify-content: center;
    padding-top: 0.9375rem;
    margin-bottom: 0.75rem;
    li {
      margin-left: 1.5rem;
      a {
        color: #6c88a6;
      }
    }
    li:nth-of-type(1) {
      margin-left: 0;
    }
  }
  p {
    text-align: center;
    margin-bottom: 0.5rem;
    img {
      width: 1.875rem;
      margin-right: 0.3125rem;
    }
    a {
      color: #6c88a6;
      text-decoration: underline;
    }
  }
}
</style>
