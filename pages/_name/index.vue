<template>
  <div id="home">
    <header class="headernav">
      <img src="~/assets/logo.png" alt class="logo" />
      <div class="input">
        <p @click="goadd">
          {{ cityname }}
          <img src="~/assets/downsan.png" alt />
          <span>|</span>
        </p>
        <img class="sou" src="~/assets/search.png" alt />
        <nuxt-link :to="'/' + jkl + '/searchname'">
          <input type="text" placeholder="请输入楼盘名" disabled />
        </nuxt-link>
      </div>
      <nuxt-link :to="'/' + jkl + '/home'">
        <img src="~/assets/index-peo.png" alt class="peo" v-if="isw" />
        <img src="~/assets/index-peoed.png" alt class="peo" v-if="!isw" />
      </nuxt-link>
    </header>
    <div class="headerbox">
      <img src="~/assets/index-topimg.jpg" alt="" class="topimg" />
    </div>
    <div class="topnav">
      <div class="navs n1">
        <ul class="bold b1">
          <li>
            <nuxt-link :to="'/' + jkl + '/search'">
              <img src="~/assets/normal-search.png" alt />
            </nuxt-link>
            <p>新房</p>
          </li>
          <li>
            <nuxt-link :to="'/' + jkl + '/search/near_railway-1'">
              <img src="~/assets/normal-static.png" alt />
            </nuxt-link>
            <p>地铁房</p>
          </li>
          <li>
            <nuxt-link :to="'/' + jkl + '/special'">
              <img src="~/assets/normal-special.png" alt />
            </nuxt-link>
            <p>特价房</p>
          </li>
          <li>
            <nuxt-link :to="'/' + jkl + '/map'">
              <img src="~/assets/normal-map.png" alt />
            </nuxt-link>
            <p>地图找房</p>
          </li>
          <li>
            <nuxt-link :to="'/' + jkl + '/join'">
              <img src="~/assets/normal-join.png" alt />
            </nuxt-link>
            <p>平台合作</p>
          </li>
        </ul>
        <ul class="b2">
          <li>
            <nuxt-link :to="'/' + jkl + '/help'">
              <img src="~/assets/normal-help.png" alt />
            </nuxt-link>
            <p>帮我找房</p>
          </li>
          <li>
            <nuxt-link :to="'/' + jkl + '/weike/before/56'">
              <img src="~/assets/normal-weike.png" alt />
            </nuxt-link>
            <p>百科</p>
          </li>
          <li>
            <nuxt-link :to="'/' + jkl + '/infos/46'">
              <img src="~/assets/normal-infos.png" alt />
            </nuxt-link>
            <p>资讯</p>
          </li>
          <li>
            <nuxt-link :to="'/' + jkl + '/dynamics'">
              <img src="~/assets/normal-dynamic.png" alt />
            </nuxt-link>
            <p>新盘动态</p>
          </li>
          <li @click="zhaopin">
            <img src="~/assets/index-zhao.png" alt />
            <p>招聘英才</p>
          </li>
        </ul>
        <div class="line"></div>
        <div class="swipe">
          <img src="~/assets/index-tit.png" alt class="swipe-logo" />
          <notice-bar :scrollable="false" background="#fff" color="#646566">
            <swipe
              vertical
              class="notice-swipe"
              :autoplay="3000"
              :show-indicators="false"
            >
              <swipe-item v-for="(item, key) in tops" :key="key">
                <nuxt-link :to="'/' + jkl + '/info/' + item.id">{{
                  item.title
                }}</nuxt-link>
              </swipe-item>
            </swipe>
          </notice-bar>
        </div>
      </div>
    </div>

    <div class="help" @click="gohelp">
      <button>帮我找房</button>
    </div>
    <div class="te" v-if="discounts.length">
      <div class="top">
        热销房源
        <nuxt-link :to="'/' + jkl + '/search'">
          <span>
            全部楼盘
            <img src="~/assets/j-more.png" alt />
          </span>
        </nuxt-link>
      </div>
      <div class="swiper-te">
        <div class="swiper-wrapper">
          <div class="swiper-slide" v-for="(item, key) in discounts" :key="key">
            <nuxt-link :to="'/' + jkl + '/content/' + item.id">
              <img :src="item.img" :alt="item.name" :title="item.name" />
              <div class="strict-bom">
                <h6>{{ item.name }}</h6>
                <div class="te-bbom">
                  <p>
                    {{item.price}}
                    <span>元/m²</span>
                  </p>
                  <i>{{ item.country }}</i>
                </div>
              </div>
            </nuxt-link>
          </div>
        </div>
      </div>
    </div>
    <!-- 每周热榜 -->
    <div class="weekhot">
      <h2>
        <img src="~/assets/index-hot.jpg" alt="" class="hot" />
        每周热榜
        <nuxt-link :to="'/' + jkl + '/top/0'">
          <span>全部榜单<img src="~/assets/j-more.png" alt="" /></span>
        </nuxt-link>
      </h2>
      <div class="swiper-hot">
        <div class="swiper-wrapper">
          <div class="swiper-slide">
            <img src="~/assets/index-bei.jpg" alt="" class="bei" />
            <img
              :src="rigid_demands[0].img"
              alt=""
              class="hot-bg"
              v-if="rigid_demands.length"
            />
            <h4>刚需楼盘榜</h4>
            <p class="time">更新于{{ time }}</p>
            <ul>
              <nuxt-link
                v-for="item in rigid_demands"
                :key="item.id"
                :to="'/' + jkl + '/content/' + item.id"
              >
                <li>
                  <p></p>
                  {{ item.name }}
                </li>
              </nuxt-link>
            </ul>
            <div class="bom">
              <nuxt-link :to="'/' + jkl + '/top/0'">
                <button>
                  更多榜单<img src="~/assets/index-round.png" alt="" />
                </button>
              </nuxt-link>
            </div>
            <div class="bombtn">456456456</div>
          </div>
          <div class="swiper-slide">
            <img src="~/assets/index-bei.jpg" alt="" class="bei" />
            <img
              :src="investments[0].img"
              alt=""
              class="hot-bg"
              v-if="investments.length"
            />
            <h4>投资楼盘榜</h4>
            <p class="time">更新于{{ time }}</p>
            <ul>
              <nuxt-link
                v-for="item in investments"
                :key="item.id"
                :to="'/' + jkl + '/content/' + item.id"
              >
                <li>
                  <p></p>
                  {{ item.name }}
                </li>
              </nuxt-link>
            </ul>
            <div class="bom">
              <nuxt-link :to="'/' + jkl + '/top/1'">
                <button>
                  更多榜单<img src="~/assets/index-round.png" alt="" />
                </button>
              </nuxt-link>
            </div>
            <div class="bombtn">456456456</div>
          </div>
          <div class="swiper-slide">
            <img src="~/assets/index-bei.jpg" alt="" class="bei" />
            <img
              :src="improvements[0].img"
              alt=""
              class="hot-bg"
              v-if="improvements.length"
            />
            <h4>改善楼盘榜</h4>
            <p class="time">更新于{{ time }}</p>
            <ul>
              <nuxt-link
                v-for="item in improvements"
                :key="item.id"
                :to="'/' + jkl + '/content/' + item.id"
              >
                <li>
                  <p></p>
                  {{ item.name }}
                </li>
              </nuxt-link>
            </ul>
            <div class="bom">
              <nuxt-link :to="'/' + jkl + '/top/2'">
                <button>
                  更多榜单<img src="~/assets/index-round.png" alt="" />
                </button>
              </nuxt-link>
            </div>
            <div class="bombtn">456456456</div>
          </div>
          <div class="swiper-slide">
            <img src="~/assets/index-bei.jpg" alt="" class="bei" />
            <img
              :src="existing_houses[0].img"
              alt=""
              class="hot-bg"
              v-if="existing_houses.length"
            />
            <h4>现房楼盘榜</h4>
            <p class="time">更新于{{ time }}</p>
            <ul>
              <nuxt-link
                v-for="item in existing_houses"
                :key="item.id"
                :to="'/' + jkl + '/content/' + item.id"
              >
                <li>
                  <p></p>
                  {{ item.name }}
                </li>
              </nuxt-link>
            </ul>
            <div class="bom">
              <nuxt-link :to="'/' + jkl + '/top/3'">
                <button>
                  更多榜单<img src="~/assets/index-round.png" alt="" />
                </button>
              </nuxt-link>
            </div>
            <div class="bombtn">456456456</div>
          </div>
        </div>
      </div>
    </div>
    <div class="dynamics">
      <h2>
        楼盘动态<img src="~/assets/content-tt.png" alt="" class="tab" />
        <nuxt-link :to="'/' + jkl + '/dynamics'">
          <span class="rit"
            >全部动态<img src="~/assets/j-more.png" alt=""
          /></span>
        </nuxt-link>
      </h2>
      <div class="dynamicsbox">
        <nuxt-link :to="'/' + jkl + '/dynamic/' + dynamics[0].id">
          <div class="left">
            <div class="left-top">
              <img :src="dynamics[0].img" alt="" />
              <p>{{ dynamics[0].name }}</p>
            </div>
            <div class="left-bom">
              <p>{{ dynamics[0].title }}</p>
            </div>
          </div>
        </nuxt-link>
        <div class="right">
          <nuxt-link :to="'/' + jkl + '/dynamic/' + dynamics[1].id">
            <div class="top">
              <div class="msg">
                <p>{{ dynamics[1].title }}</p>
              </div>
              <div class="img">
                <img :src="dynamics[1].img" alt="" />
                <p>{{ dynamics[1].name }}</p>
              </div>
            </div>
          </nuxt-link>
          <nuxt-link :to="'/' + jkl + '/dynamic/' + dynamics[2].id">
            <div class="top">
              <div class="msg">
                <p>{{ dynamics[2].title }}</p>
              </div>
              <div class="img">
                <img :src="dynamics[2].img" alt="" />
                <p>{{ dynamics[2].name }}</p>
              </div>
            </div>
          </nuxt-link>
        </div>
      </div>
    </div>
    <div class="guide">
      <h3>
        楼盘导购
        <nuxt-link :to="'/' + jkl + '/infos/46'">
          <span class="more">
            更多资讯
            <img src="~/assets/j-more.png" alt />
          </span>
        </nuxt-link>
      </h3>
      <ul>
        <template v-for="(item, key) in articles">
          <nuxt-link
            :key="key"
            :to="'/' + jkl + '/info/' + item.id"
            v-if="key < 3"
          >
            <li>
              <div class="left">
                <p class="tit">{{ item.title }}</p>
                <p class="time">
                  <span>{{ item.source }}</span>
                  {{ item.time }}
                </p>
              </div>
              <img :src="item.img" :alt="item.title" :title="item.title" />
            </li>
          </nuxt-link>
        </template>
      </ul>
    </div>
    <div class="other">
      <h1>推荐房源<nuxt-link :to="'/'+jkl+'/search'"><span>全部楼盘<img src="~/assets/content-join.png" alt=""></span></nuxt-link></h1>
      <template v-for="(item, key) in recommends">
        <nuxt-link :key="key" :to="'/' + jkl + '/content/' + item.id">
          <div class="pro">
            <img :src="item.img" :alt="item.name" :title="item.name" />
            <div class="pro-msg">
              <h5>
                {{ item.name }}
                <span :class="item.state == '待售' ? 'null' : ''">{{
                  item.state
                }}</span>
              </h5>
              <p class="pro-price">
                <span>{{ item.price }}</span>
                <i>元/m²</i>
              </p>
              <p class="attr">
                {{ item.type }} | {{ item.city }}-{{
                  item.country.substr(0, 2)
                }}
                | {{ item.area }}m²
              </p>
              <p class="pro-icon">
                <span class="pro-icon-zhuang">{{ item.decorate }}</span>
                <span
                  class="pro-icon-type"
                  v-for="(val, k) in item.feature"
                  :key="k"
                  >{{ val }}</span
                >
              </p>
            </div>
          </div>
        </nuxt-link>
      </template>
    </div>
    <nuxt-link :to="'/' + jkl + '/search'">
      <button class="morebtn">
        更多楼盘<img src="~/assets/index-more.png" alt="" />
      </button>
    </nuxt-link>
  </div>
</template>
<script>
import "@/static/css/foot.css";
import { NoticeBar, Swipe, SwipeItem } from "vant";
import Swiper from "swiper";
import "swiper/css/swiper.min.css";
export default {
  components: {
    NoticeBar,
    Swipe,
    SwipeItem,
  },
  async asyncData(context) {
    try {
      let city = context.store.state.city;
      let host = context.store.state.host;
      let jkl = context.params.name;
      let [res] = await Promise.all([
        context.$axios
          .get("/edefang/mobile", {
            params: {
              city: city,
            },
          })
          .then((resp) => {
            let data = resp.data.data;
            // console.log(data)
            // console.log(host);
            return data;
          }),
      ]);
      return {
        tops: res.tops,
        stricts: res.stricts,
        finishes: res.finishes,
        articles: res.guides,
        recommends: res.recommend,
        discounts: res.hot_deals,
        dynamics: res.dynamics,
        jkl: jkl,
        cityname: res.current_city.name,
        city: city,
        title: res.common.header.title,
        description: res.common.header.description,
        keywords: res.common.header.keywords,
        host: host,
        banner: res.banner,
        rigid_demands: res.characteristics.rigid_demands,
        improvements: res.characteristics.improvements,
        investments: res.characteristics.investments,
        existing_houses: res.characteristics.existing_houses,
      };
    } catch (err) {
      console.log("errConsole========:", err);
      context.error({ statusCode: 404, message: "页面未找到或无数据" });
    }
  },
  head() {
    return {
      title: this.title || "易得房-" + this.cityname,
      meta: [
        {
          name: "description",
          content: this.description || "易得房",
        },
        {
          name: "Keywords",
          content: this.keywords || "易得房",
        },
      ],
    };
  },
  data() {
    return {
      jkl: "",
      tops: [],
      stricts: [],
      finishes: [],
      articles: [],
      recommends: [],
      discounts: [],
      dynamics: [],
      cityname: "",
      host: 0,
      txt: "家园",
      time: "",
      isw: true,
    };
  },
  methods: {
    zhaopin() {
      window.location.href =
        "http://recruit.jy1980.com?type=1&city=" + localStorage.getItem("city");
    },
    goback() {
      window.location.href = this.banner.url;
    },
    gohelp() {
      this.$router.push("/" + this.jkl + "/help");
    },
    goadd() {
      this.$router.push("/" + this.jkl + "/address");
    },
    goluck() {
      this.$router.push("/" + this.jkl + "/lucky");
    },
    settop() {
      var scrollTop = window.scrollY;
      // console.log(scrollTop)
      if (scrollTop >= 10) {
        $(".headernav").css({ background: "rgba(255,255,255,1)" });
        this.isw = false;
      } else {
        $(".headernav").css({ background: "rgba(0,0,0,0)" });
        this.isw = true;
      }
    },
  },
  mounted() {
    let date = new Date();
    this.time =
      date.getFullYear() +
      "-" +
      (date.getMonth() + 1 > 10
        ? date.getMonth() + 1
        : "0" + (date.getMonth() + 1)) +
      "-" +
      (date.getDate() > 10 ? date.getDate() : "0" + date.getDate());
    window.addEventListener("scroll", this.settop);
    if (this.host == 0) {
      this.txt = "家园";
    } else {
      this.txt = "易得房";
    }
    console.log(this.$store.state);
    // this.cityname = $cookies.get('cityname')
    localStorage.setItem("cityname", this.cityname);
    var swiper07 = new Swiper(".swiper-nav", {
      slidesPerView: 5,
      spaceBetween: 0,
      resistanceRatio: 0,
      slidesOffsetAfter: 0,
      slidesOffsetBefore: 0,
      pagination: {
        el: ".swiper-pagination1",
      },
    });
    var swiper08 = new Swiper(".swiper-dynamic", {
      slidesPerView: 1,
      spaceBetween: 10,
      slidesOffsetAfter: 0,
      slidesOffsetBefore: 0,
      pagination: {
        el: ".swiper-pagination",
      },
    });
    var swiper09 = new Swiper(".swiper-te", {
      slidesPerView: 2.32,
      spaceBetween: 10,
      slidesOffsetAfter: 14,
      slidesOffsetBefore: 15,
    });
    var swiper10 = new Swiper(".swiper-hot", {
      slidesPerView: 1.45,
      spaceBetween: 15,
      slidesOffsetAfter: 20,
      slidesOffsetBefore: 15,
    });
  },
  beforeDestroy() {
    window.removeEventListener("scroll", this.settop);
  },
};
</script>
<style lang="less" scoped>
#home {
  background: #f5f5f5;
}
li {
  list-style: none;
}
header {
  height: 3.4375rem;
  padding: 0 4%;
  line-height: 3.4375rem;
  position: fixed;
  width: 92%;
  z-index: 5;
  display: flex;
  .logo {
    width: 3.375rem;
    margin-right: 0.625rem;
    height: 1.5rem;
    margin-top: 1rem;
  }
  .input {
    width: 15rem;
    height: 2.125rem;
    border-radius: 1.0625rem;
    background-color: #f5f5f5;
    margin-top: 0.625rem;
    margin-right: 0.9375rem;
    display: flex;
    align-items: center;
    p {
      color: #303234;
      font-size: 0.875rem;
      padding-left: 0.625rem;
      img {
        width: 0.75rem;
      }
      span {
        color: #d5d6df;
      }
    }
    .sou {
      width: 1rem;
      height: 1rem;
      margin-left: 0.25rem;
    }
  }
  input {
    width: 6.25rem;
    border: 0;
    outline: none;
    background-color: #f5f5f5;
    margin-left: 0.375rem;
    height: 2.125rem;
  }
  .peo {
    width: 1.5rem;
    height: 1.5rem;
    margin-top: 0.875rem;
  }
  .topimg {
    width: 100%;
    border-radius: 0.75rem;
  }
}
.headerbox {
  padding: 0 4%;
  height: 6.25rem;
  background: url(~assets/index-bg.png);
  background-size: 100%;
  background-repeat: no-repeat;
  padding-top: 3.4375rem;
  .topimg {
    width: 100%;
    border-radius: 0.75rem;
  }
}
.topnav {
  padding: 0 4%;
  padding-top: 0.625rem;
  .navs {
    margin-bottom: 0.625rem;
    background-color: #fff;
    border-radius: 0.75rem;
    overflow: hidden;
  }
  .n1 {
    padding-top: 0.875rem;
  }
  ul {
    padding: 0 1rem;
    display: flex;
    justify-content: space-between;
    padding-top: 0.1875rem;
    li {
      // width: 20%;
      width: 3.1rem;
      text-align: center;
      position: relative;
      img {
        width: 2.75rem;
        height: 2.75rem;
      }
      p {
        color: #313233;
        font-size: 0.75rem;
        position: relative;
        top: -0.1875rem;
      }
      span {
        display: block;
        position: absolute;
        width: 1.75rem;
        height: 0.875rem;
        background: #ff4040;
        border-radius: 0.4375rem 0.4375rem 0.4375rem 0px;
        color: #fef2f2;
        font-size: 0.625rem;
        text-align: center;
        line-height: 0.875rem;
        right: 0;
        top: 0;
      }
    }
  }
  .bold {
    p {
      font-weight: bold;
    }
  }
  .bold1 {
    margin-bottom: 0.625rem;
  }
  .b1 {
    margin-bottom: 0.4375rem;
    li {
      img {
        margin-bottom: 0.5625rem;
      }
    }
  }
  .b2 {
    padding-bottom: 0.4375rem;
    li {
      img {
        width: 1.5rem;
        height: 1.5rem;
        margin-bottom: 0.5625rem;
      }
    }
  }
  .line {
    background: #ededed;
    height: 0.0625rem;
    width: 19.6875rem;
    margin-left: 0.9375rem;
  }
  .swipe {
    display: flex;
    height: 2.8125rem;
    align-items: center;
    position: relative;
    /deep/.van-notice-bar__wrap {
      width: 100%;
    }
    .van-swipe {
      width: 100%;
    }
    .van-notice-bar {
      width: 100%;
    }
    .notice-swipe {
      height: 40px;
      line-height: 40px;
    }
    .van-swipe-item {
      width: 16.25rem;
      overflow: hidden;
      text-overflow: ellipsis;
      white-space: nowrap;
      a {
        color: #646566;
        font-size: 0.875rem;
      }
    }
    .swipe-logo {
      margin-left: 0.9375rem;
      width: 3.75rem;
    }
  }
}

.help {
  margin: 0 4%;
  height: 3.75rem;
  margin-bottom: 0.625rem;
  background: url(~assets/index-help.jpg) no-repeat;
  background-size: 100%;
  border-radius: 0.75rem;
  display: flex;
  align-items: center;
  button {
    margin-left: auto;
    margin-right: 1.25rem;
    width: 4.625rem;
    height: 1.75rem;
    border-radius: 0.875rem;
    text-align: center;
    line-height: 1.75rem;
    background-color: #fcd19e;
    border: 0;
    color: #4c4a41;
    font-size: 0.8125rem;
    box-shadow: -1.5px 2px 5px 0px rgba(75, 211, 182, 0.2);
  }
}
.te {
  margin: 0 4%;
  height: 13rem;
  margin-bottom: 0.625rem;
  border-radius: 0.75rem;
  background-color: #fff;
  .top {
    padding-top: 1.0625rem;
    margin-bottom: 0.875rem;
    padding-left: 0.9375rem;
    font-size: 1rem;
    color: #17181a;
    font-weight: 800;
    display: flex;
    align-items: center;
    a {
      margin-left: auto;
      margin-right: 0.875rem;
    }
    span {
      color: #7d7f80;
      font-size: 0.75rem;
      img {
        width: 0.75rem;
        height: 0.75rem;
        margin-bottom: -0.0625rem;
      }
    }
  }
  .swiper-te {
    overflow: hidden;
    padding-bottom: 1.4375rem;
    .swiper-slide {
      border: 0.03125rem solid #f0f1f5;
      box-shadow: 0.03125rem 0.15625rem 0.625rem 0px rgba(0, 0, 0, 0.05);
      border-radius: 0.375rem;
      height: 8.375rem;
      overflow: hidden;
      img {
        width: 100%;
        height: 5rem;
      }
      .strict-bom {
        padding-left: 0.5rem;
        h6 {
          color: #4d4d4d;
          font-size: 0.875rem;
          margin-top: 0.1875rem;
          margin-bottom: 0;
          overflow: hidden;
          text-overflow: ellipsis;
          white-space: nowrap;
        }
        .te-bbom {
          display: flex;
          align-items: center;
        }
        p {
          color: #ff5454;
          font-size: 0.75rem;
          display: inline-block;
          font-weight: bold;
          span {
            font-size: 0.625rem;
          }
        }
        i {
          margin-left: auto;
          font-style: normal;
          color: #96989a;
          font-size: 0.6875rem;
          margin-right: 0.5rem;
        }
      }
    }
  }
}
.weekhot {
  margin: 0 0.9375rem;
  border-radius: 0.75rem;
  background-color: #fff;
  padding-top: 1.125rem;
  padding-bottom: 1.125rem;
  margin-bottom: 0.625rem;
  h2 {
    display: flex;
    color: #17181a;
    font-size: 1rem;
    display: flex;
    padding-left: 1.125rem;
    .hot {
      width: 1.3125rem;
      height: 1.3125rem;
      margin-right: 0.25rem;
    }
    a {
      margin-left: auto;
    }
    span {
      margin-left: auto;
      color: #7d7f80;
      font-size: 0.75rem;
      margin-right: 0.9375rem;
      font-style: normal;
      img {
        width: 0.625rem;
        margin-left: 0.25rem;
      }
    }
  }
  .swiper-hot {
    padding-top: 1.125rem;
    overflow: hidden;
    .swiper-slide {
      height: 12.5rem;
      border-radius: 0.375rem;
      background: #f5f7f7;
      position: relative;
      .bei {
        width: 1.625rem;
        border-radius: 0.375rem 0 0.375rem 0;
        position: absolute;
        left: 0;
        top: 0;
      }
      .hot-bg {
        width: 5.625rem;
        height: 4.125rem;
        border-radius: 0.1875rem;
        position: absolute;
        right: -0.3125rem;
        top: -0.375rem;
      }
      h4 {
        color: #af772d;
        font-size: 0.875rem;
        padding-left: 2.25rem;
        padding-top: 0.625rem;
        margin-bottom: 0.5rem;
      }
      .time {
        color: #646466;
        font-size: 0.625rem;
        padding-left: 0.875rem;
        margin-bottom: 1.3rem;
      }
      ul {
        li {
          display: flex;
          align-items: center;
          padding-left: 0.875rem;
          color: #4b4b4d;
          font-size: 0.875rem;
          margin-bottom: 0.5rem;
          p {
            width: 0.25rem;
            height: 0.25rem;
            border-radius: 50%;
            background: #af772d;
            margin-right: 0.375rem;
          }
        }
      }
      .bom {
        position: absolute;
        bottom: 0;
        width: 100%;
        height: 2.5rem;
        left: 0;
        display: flex;
        justify-content: center;
        z-index: 2;
        button {
          width: 9.375rem;
          height: 2.5rem;
          display: flex;
          justify-content: center;
          align-items: center;
          border: 0;
          outline: none;
          color: #737980;
          font-size: 0.75rem;
          background: #f5f7f7;
          img {
            width: 0.75rem;
            margin-left: 0.3125rem;
          }
        }
      }
      .bombtn {
        width: 9.375rem;
        height: 2.5rem;
        box-shadow: 0px -0.25rem 0.46875rem -0.25rem rgba(4, 0, 0, 0.05);
        position: absolute;
        bottom: 0;
        left: 50%;
        transform: translateX(-50%);
      }
    }
  }
}
.dynamics {
  margin: 0 0.9375rem;
  border-radius: 0.75rem;
  background-color: #fff;
  padding: 0 0.9375rem 0.625rem 0.9375rem;
  h2 {
    padding-top: 1.125rem;
    color: #17181a;
    font-size: 1.0625rem;
    margin-bottom: 1rem;
    position: relative;
    display: flex;
    margin-bottom: 1rem;
    .tab {
      position: relative;
      font-weight: normal;
      top: -0.125rem;
      width: 2rem;
      height: 1rem;
    }
    a {
      margin-left: auto;
    }
    .rit {
      color: #969799;
      font-size: 0.8125rem;
      font-weight: normal;
      img {
        width: 0.75rem;
        margin-left: 0.375rem;
      }
    }
  }
  .dynamicsbox {
    display: flex;
    .left {
      width: 8.28125rem;
      height: 8.875rem;
      border-radius: 0.375rem;
      overflow: hidden;
      margin-right: 0.625rem;
      .left-top {
        height: 4.75rem;
        position: relative;
        img {
          width: 100%;
          height: 100%;
        }
        p {
          color: #ffffff;
          font-size: 0.8125rem;
          position: absolute;
          left: 0.625rem;
          bottom: 0.5rem;
          font-weight: 800;
        }
      }
      .left-bom {
        height: 4.125rem;
        background-color: #f5f6f7;
        font-size: 0.75rem;
        line-height: 1.0625rem;
        padding: 0 0.625rem;
        display: flex;
        align-items: center;
        color: #646466;
        p {
          display: -webkit-box;
          -webkit-box-orient: vertical;
          -webkit-line-clamp: 2;
          overflow: hidden;
        }
      }
    }
    .right {
      width: 10.78125rem;
      .top {
        display: flex;
        border-radius: 0.375rem;
        background: #f5f6f7;
        height: 4.125rem;
        margin-bottom: 0.625rem;
        overflow: hidden;
        .msg {
          width: 6.75rem;
          height: 4.125rem;
          display: flex;
          align-items: center;
          color: #646466;
          font-size: 0.75rem;
          justify-content: center;
          p {
            display: -webkit-box;
            -webkit-box-orient: vertical;
            -webkit-line-clamp: 2;
            overflow: hidden;
            width: 5.125rem;
          }
        }
        .img {
          width: 4rem;
          height: 4.125rem;
          position: relative;
          img {
            width: 100%;
            height: 100%;
          }
          p {
            position: absolute;
            color: #ffffff;
            font-size: 0.8125rem;
            left: 50%;
            transform: translateX(-50%);
            top: 1rem;
            width: 3.125rem;
          }
        }
      }
    }
  }
}
.guide {
  margin: 0 0.9375rem;
  padding: 1.1rem 0.9375rem 0.625rem 0.9375rem;
  border-radius: 0.75rem;
  background-color: #fff;
  margin-top: 10px;
  h3 {
    color: #323333;
    font-size: 1rem;
    margin-bottom: 1.125rem;
    .more {
      float: right;
      color: #7d7f80;
      font-weight: 400;
      font-size: 0.75rem;
      img {
        width: 0.75rem;
        margin-left: 0.25rem;
        margin-bottom: -0.0625rem;
      }
    }
  }
  ul {
    li {
      display: flex;
      margin-bottom: 0.625rem;
      .left {
        width: 14.0625rem;
        position: relative;
        .tit {
          color: #323333;
          font-size: 0.9375rem;
          line-height: 1.25rem;
          margin-bottom: 0.75rem;
          display: -webkit-box;
          -webkit-box-orient: vertical;
          -webkit-line-clamp: 2;
          overflow: hidden;
          height: 2.375rem;
          position: relative;
          top: -0.1875rem;
        }
        .time {
          color: #969899;
          font-size: 0.6875rem;
          position: absolute;
          bottom: 0.4375rem;
          span {
            margin-right: 0.25rem;
          }
        }
      }
      img {
        width: 6.25rem;
        height: 4.375rem;
        border: 0.1875rem;
        margin-left: 1.25rem;
        border-radius: 0.375rem;
      }
    }
  }
}
.other {
  padding: 1.125rem 0.9375rem 0.9375rem 0.9375rem;
  margin: 0 4%;
  margin-bottom: 0.9375rem;
  border-radius: 0.75rem;
  background-color: #fff;
  margin-top: 10px;
  h1 {
    color: #17181a;
    font-size: 1rem;
    margin-bottom: 0.9375rem;
    span {
      color: #7D7F80;
      font-size: .75rem;
      float: right;
      img {
        width: .75rem;
        margin-left: .25rem;
        margin-bottom: -0.125rem;
      }
    }
  }
  .pro {
    margin-bottom: 1.875rem;
    width: 100%;
    display: flex;
    text-decoration: none;
    img {
      width: 6.875rem;
      height: 5rem;
      margin-right: 0.75rem;
      border-radius: 0.375rem;
    }
    .pro-msg {
      flex: 1;
      h5 {
        color: #323233;
        font-size: 1rem;
        font-weight: bold;
        margin-top: -0.25rem;
        margin-bottom: 0.125rem;
        span {
          color: #b68826;
          font-size: 0.6875rem;
          float: right;
          padding: 0.1875rem 0.375rem;
          background-color: #f8efdc;
          border-radius: 0.125rem;
          font-weight: 400;
        }
        .null {
          background: #eaf1f9;
          color: #759ecb;
        }
      }
      .pro-price {
        color: #7a7a7a;
        font-size: 0.75rem;
        margin-bottom: 0.1875rem;
        span {
          color: #ff5454;
          font-size: 0.9375rem;
          font-weight: bold;
        }
        i {
          font-style: normal;
          color: #ff5454;
        }
      }
      .attr {
        color: #7a7a7a;
        font-size: 0.75rem;
        margin-bottom: 0.1875rem;
      }
      .pro-icon {
        display: flex;
        .pro-icon-zhuang {
          color: #5aabe5;
          font-size: 0.6875rem;
          padding: 0.1875rem 0.375rem;
          background-color: #f0f5f9;
          margin-right: 0.375rem;
          border-radius: 0.125rem;
        }
        .pro-icon-type {
          color: #888a8f;
          font-size: 0.6875rem;
          padding: 0.1875rem 0.375rem;
          border-radius: 0.125rem;
          background-color: #f7f8fa;
          margin-right: 0.375rem;
        }
      }
    }
  }
  a:last-child {
    .pro {
      margin-bottom: 0;
    }
  }
}
.morebtn {
  width: 92%;
  margin-left: 4%;
  height: 2.875rem;
  border-radius: 0.75rem;
  text-align: center;
  line-height: 2.875rem;
  background-color: #ffe3a9;
  border: 0;
  margin-bottom: 1.625rem;
  color: #b68826;
  font-size: 0.875rem;
  img {
    width: 0.75rem;
    margin-left: 0.25rem;
    margin-bottom: -0.0625rem;
  }
}
</style>
