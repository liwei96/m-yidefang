<template>
  <div id="hu">
    <top-view :jkl="jkl"></top-view>
    <div class="topimg" @click="big(now.small)">
      <img :src="now.small" :alt="now.title" :title="title + now.title" />
      <p>共1张</p>
    </div>
    <div class="msg">
      <h3>
        {{ now.title }}
        <span>{{ now.state }}</span>
      </h3>
      <p class="pri">
        约
        <span>{{ now.price }}</span
        >万/套
      </p>
      <div class="msgli">
        <div class="left">
          建面：
          <span>{{ now.area }}m²</span>
        </div>
        <div class="left">
          层高：
          <span>{{ Number(now.height) }}m</span>
        </div>
      </div>
      <div class="msgli">
        <div class="left">
          类型：
          <span>{{ now.type }}</span>
        </div>
        <div class="left">
          特色：
          <span>{{ now.special }}</span>
        </div>
      </div>
      <!-- <div class="msgli tese">
        <div class="left">
          特色：
          <span>{{ now.special }}</span>
        </div>
      </div> -->
      <nuxt-link :to="'/' + jkl + '/rim/' + now.bid">
        <p class="address">
          <img class="path" src="~/assets/icon-path.png" alt />
          <span>{{ now.address }}</span>
          <img src="~/assets/j-more.png" alt class="more" />
        </p>
      </nuxt-link>
      <div class="btn">
        <button @click="pop('领取全部户型资料', 56, '详情页+领取全部户型资料')">
          咨询详细户型
        </button>
        <button @click="pop('查询最底价', 34, '详情页+查询最底价')">
          咨询楼盘底价
        </button>
      </div>
    </div>
    <div class="line"></div>
    <div class="ana">
      <h3>{{ txt }}咨询师</h3>
      <p class="xun-icon">
        <span> <img src="~/assets/save.png" alt />专业服务 </span>
        <span> <img src="~/assets/icon-path.png" alt />区域解读 </span>
        <span> <img src="~/assets/icon-pin.png" alt />户型分析 </span>
      </p>
      <div class="peo">
        <img :src="staff.head_img" alt />
        <div class="peomsg">
          <h5>
            {{ staff.name }}
            <span>满意度{{ staff.num }}分</span>
          </h5>
          <p>
            带看{{ staff.ServeNum }}次，已有
            <span>{{ Number(staff.ServeNum) + 44 }}</span
            >人咨询
          </p>
        </div>
        <button @click="pop('一键咨询', 84, '详情页+一键咨询')">
          一键咨询
        </button>
      </div>
      <h4>户型分析</h4>
      <p class="analy" v-html="now.analysis"></p>
    </div>
    <div class="line"></div>
    <div class="hui">
      <h3>
        优惠信息
        <span @click="huo = true">
          <img src="~/assets/ques.png" alt /> 活动规则
        </span>
      </h3>
      <div class="hui-con">
        <div class="hui-left">
          <p>
            售楼处专供易得房客户<span>（{{ hui.dead_line }}截止）</span>
          </p>
        </div>
        <div class="hui-right">
          <button @click="pop('领取优惠', 94, '详情页+领取优惠')">
            领取优惠
          </button>
          <p>
            <span>{{ hui.receive_num }}人</span>已领取
          </p>
        </div>
      </div>
      <div class="hui-con two">
        <div class="hui-left">
          <p>
            免费专车1对1服务限时劵<span>（剩余{{ hui.remain_num }}张）</span>
          </p>
        </div>
        <div class="hui-right">
          <button @click="pop('免费专车看房', 86, '详情页+免费专车看房')">
            免费领取
          </button>
          <p>
            <span>{{ hui.receive_num }}人</span>已领取
          </p>
        </div>
      </div>
    </div>
    <div class="line"></div>
    <div class="con" v-if="other.length">
      <h3>本楼盘其它户型</h3>
      <template v-for="(item, key) in other">
        <nuxt-link :key="key" :to="'/' + jkl + '/hu/' +$route.params.pro+'/'+ item.id">
          <div class="li">
            <div class="left">
              <img
                :src="item.small"
                :alt="title + item.title"
                :title="title + item.title"
              />
            </div>
            <div class="right">
              <h4>
                {{ item.title }}
                <span>{{ item.state }}</span>
              </h4>
              <p>
                建面：
                <span>{{ item.area }}m²</span>
              </p>
              <p>
                类型：
                <span>{{ item.type }}</span>
              </p>
              <p class="total">
                总价：
                <span>
                  约
                  <i>{{ item.price }}</i
                  >万/套
                </span>
              </p>
            </div>
          </div>
        </nuxt-link>
      </template>
    </div>
    <div class="line"></div>
    <div class="other">
      <h3>看了该楼盘的还看了</h3>
      <template v-for="(item, key) in recommends">
        <nuxt-link :key="key" :to="'/' + jkl + '/content/' + item.id">
          <div class="pro">
            <img :src="item.img" :alt="item.name" :title="item.name" />
            <div class="pro-msg">
              <h5>
                {{ item.name }}
                <span>{{ item.status }}</span>
              </h5>
              <p class="pro-price">
                <span>{{ item.single_price }}</span>
                <i>元/m²</i>起
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
                  v-for="(val, k) in item.features"
                  :key="k"
                  >{{ val }}</span
                >
              </p>
            </div>
          </div>
        </nuxt-link>
      </template>
    </div>
    <nav-view :phone="phone" @fot="chang($event)"></nav-view>
    <van-popup
      v-model="tan"
      :style="{ background: 'rgba(0,0,0,0)' }"
      @click-overlay="typebtn = 0"
    >
      <tan-view
        :txt="remark"
        :typenum="typenum"
        :id="id"
        :name="name"
        @close="cli($event)"
        :typebtn="typebtn"
        :proname="now.name"
      ></tan-view>
    </van-popup>
    <van-popup v-model="huo" :style="{ background: 'rgba(0,0,0,0)' }">
      <div class="huo-msg">
        <div class="msg-con">
          <h4>活动规则</h4>
          <img @click="huo = false" src="~/assets/w-del.png" alt />
          <div>
            <p>
              平台优惠发放时间：待开发商或总代理公司补贴发放到位后尽快发放。
            </p>
            <p>
              核算方式：由开发商或代理公司判定为易得房平台客户即可享受这个优惠。
            </p>
            <p>
              结算方式：提供已实名的支付宝账户给与您对接的易得房咨询师，规定时间内会将优惠费用打至该账户。
            </p>
            <p>详细活动方案请致易得房电客服电话：4007186686</p>
            <p>注：活动最终解释权归易得房所有</p>
          </div>
        </div>
      </div>
    </van-popup>
  </div>
</template>
<script>
import { ImagePreview } from "vant";
import topView from "@/components/header.vue";
import nav from "@/components/nav.vue";
import tan from "@/components/tan.vue";
export default {
  components: {
    "top-view": topView,
    "nav-view": nav,
    [ImagePreview.Component.name]: ImagePreview.Component,
    "tan-view": tan,
  },
  async asyncData(context) {
    try {
      let host = context.store.state.host;
      let id = context.params.id;
      let pro = context.params.pro;
      let token = context.store.state.cookie.token;
      let jkl = context.params.name;
      let other = context.query.other;
      let [res] = await Promise.all([
        context.$axios
          .get("/jy/room/single", {
            params: {
              id: id,
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
        now: res.one,
        other: res.other_rooms,
        recommends: res.recommends,
        hui: res.num,
        id: pro,
        staff: res.common.staff,
        phone: res.common.phone,
        title: res.common.header.title,
        description: res.common.header.description,
        keywords: res.common.header.keywords,
        host: host,
      };
    } catch (err) {
      console.log("errConsole========:", err);
      context.error({ statusCode: 404, message: "页面未找到或无数据" });
    }
  },
  head() {
    return {
      title:
        this.title || "易得房-" + this.now.name + "-户型详情-" + this.now.title,
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
      now: {},
      other: {},
      recommends: {},
      id: 0,
      hui: {},
      tan: false,
      typenum: 0,
      typebtn: 1,
      name: "",
      remark: "",
      huo: false,
      txt: "",
    };
  },
  methods: {
    chang(data) {
      this.typenum = data.position;
      this.name = data.name;
      this.typebtn = 1;
      this.tan = true;
      this.remark = "户型页+预约看房+" + this.now.title;
      this.id = String(this.now.bid);
    },
    cli(e) {
      this.tan = e;
    },
    pop(name, position, txt) {
      this.name = name;
      this.typebtn = 1;
      this.typenum = position;
      this.tan = true;
      this.remark = txt + "+" + this.now.title;
      this.id = String(this.now.bid);
    },
    big(url) {
      let arr = [url];
      ImagePreview(arr);
    },
  },
  mounted() {
    if (this.host == 0) {
      this.txt = "家园";
    } else {
      this.txt = "易得房";
    }
  },
};
</script>
<style lang="less" scoped>
header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  height: 2.75rem;
  z-index: 1;
  background-color: #fff;
  position: fixed;
  top: 0;
  .back {
    width: 1.25rem;
    margin-left: 1rem;
  }
  .logo {
    width: 3.125rem;
  }
  .home {
    width: 1.25rem;
    margin-right: 1rem;
  }
  .zixuns {
    margin-right: 1.25rem;
    position: absolute;
    right: 9%;
    top: 0.68rem;
    img {
      width: 1.5rem;
    }
    p {
      position: absolute;
      width: 0.8125rem;
      height: 0.8125rem;
      border-radius: 50%;
      text-align: center;
      line-height: 0.8125rem;
      background-color: #ff4040;
      color: #fff;
      font-size: 0.625rem;
      top: -0.40625rem;
      right: -0.1875rem;
    }
  }
  .list {
    width: 1.25rem;
    margin-right: 4%;
  }
}
.topimg {
  padding-top: 2.75rem;
  height: 12.5rem;
  background-color: #f5f5f5;
  text-align: center;
  position: relative;
  img {
    height: 100%;
    max-width: 100%;
  }
  p {
    position: absolute;
    font-size: 0.625rem;
    color: #fff;
    text-align: center;
    line-height: 1.25rem;
    width: 2.8125rem;
    border-radius: 0.625rem;
    background: rgba(0, 0, 0, 0.6);
    right: 0.9375rem;
    bottom: 0.625rem;
  }
}
.msg {
  padding: 0 4%;
  h3 {
    color: #121212;
    font-size: 1.25rem;
    margin-top: 1.25rem;
    margin-bottom: 0.1875rem;
    span {
      color: #fff;
      font-size: 0.75rem;
      padding: 0.125rem 0.25rem;
      background-color: #d1a23d;
      border-radius: 0.125rem;
      margin-left: 0.625rem;
      font-weight: 400;
      position: relative;
      top: -0.125rem;
    }
  }
  .pri {
    color: #fe5830;
    font-size: 0.75rem;
    margin-bottom: 0.75rem;
    span {
      font-size: 1rem;
    }
  }
  .msgli {
    display: flex;
    margin-bottom: 0.3125rem;
    .left {
      color: #7d7f80;
      font-size: 0.8125rem;
      width: 50%;
      span {
        color: #323333;
        font-size: 0.875rem;
      }
    }
  }
  .tese {
    .left {
      width: 100%;
    }
  }
  .address {
    margin-top: 1.125rem;
    margin-bottom: 1.5rem;
    display: flex;
    .path {
      width: 1rem;
      height: 1rem;
      margin-right: 0.2rem;
      margin-bottom: -0.125rem;
    }
    span {
      color: #323333;
      font-size: 1rem;
      display: block;
      width: 18.75rem;
      overflow: hidden;
      text-overflow: ellipsis;
      white-space: nowrap;
      height: 0.9375rem;
      line-height: 1rem;
    }
    .more {
      float: right;
      width: 1rem;
      height: 1rem;
    }
  }
  .btn {
    margin-bottom: 1.25rem;
    button {
      width: 10.4375rem;
      height: 2.25rem;
      border-radius: 0.375rem;
      background-color: #f8efdc;
      text-align: center;
      line-height: 2.25rem;
      border: 0;
      color: #d1a23d;
      font-size: 0.9375rem;
      font-weight: bold;
      margin-right: 0.4rem;
    }
    button:nth-of-type(2) {
      margin-right: 0;
    }
  }
}
.line {
  width: 100%;
  height: 0.625rem;
  background-color: #f8f8f8;
}
.ana {
  padding: 0 4%;
  padding-bottom: 0.125rem;
  h3 {
    color: #1f1f1f;
    font-size: 1rem;
    margin-top: 1rem;
    margin-bottom: 0.875rem;
  }
  .xun-icon {
    margin-bottom: 1.25rem;
    span {
      color: #8f8f8f;
      font-size: 0.8125rem;
      margin-right: 0.625rem;
      img {
        width: 0.875rem;
        margin-right: 0.1875rem;
        margin-bottom: -0.125rem;
      }
    }
  }
  .peo {
    display: flex;
    align-items: center;
    padding-bottom: 0.9375rem;
    border-bottom: 0.03125rem solid #f3f3f3;
    img {
      width: 2.5rem;
      height: 2.5rem;
      border-radius: 50%;
      margin-right: 0.625rem;
    }
    .peomsg {
      h5 {
        color: #1f1f1f;
        font-size: 1rem;
        font-weight: 400;
        margin-bottom: 0.375rem;
        span {
          padding: 0.125rem;
          font-size: 0.625rem;
          color: #fff;
          background-color: #ff761a;
          border-radius: 0.125rem;
          margin-left: 0.25rem;
        }
      }
      p {
        color: #969899;
        font-size: 0.75rem;
        span {
          color: #ff5454;
        }
      }
    }
    button {
      width: 4.6875rem;
      height: 1.625rem;
      border-radius: 0.1875rem;
      border: 0.03125rem solid #d1a23d;
      text-align: center;
      line-height: 1.625rem;
      color: #d1a23d;
      font-size: 0.75rem;
      background-color: #f8efdc;
      margin-left: auto;
    }
  }
  h4 {
    color: #121212;
    font-size: 1.0625rem;
    margin-top: 1rem;
    margin-bottom: 0.875rem;
  }
  .analy {
    color: #7d7f80;
    font-size: 0.875rem;
    line-height: 1.3125rem;
    margin-bottom: 0.875rem;
  }
}
.hui {
  padding: 1.1875rem 4% 0.625rem 4%;
  h3 {
    color: #1f1f1f;
    font-size: 1.0625rem;
    line-height: 1.375rem;
    span {
      color: #969799;
      font-size: 0.875rem;
      float: right;
      font-weight: 400;
      img {
        width: 0.9375rem;
        margin-bottom: -0.125rem;
      }
    }
  }
  .hui-con {
    width: 100%;
    height: 4.8125rem;
    border-radius: 0.0625rem;
    background: url("~assets/b1.png") no-repeat;
    background-size: 100%;
    margin-top: 1.0625rem;
    position: relative;
    .hui-left {
      p {
        color: #e58e54;
        font-size: 0.75rem;
        position: absolute;
        bottom: 0.7rem;
        left: 8px;
      }
      span {
        color: #211c18;
        font-size: 0.625rem;
      }
    }
    .hui-right {
      position: absolute;
      top: 0.875rem;
      right: 0.9375rem;
      button {
        width: 4.375rem;
        height: 1.625rem;
        text-align: center;
        line-height: 1.625rem;
        color: #fff;
        font-size: 0.75rem;
        border: 0;
        background: linear-gradient(270deg, #ff4b2d, #ffb753);
        border-radius: 0.8125rem;
      }
      p {
        color: #ff7519;
        font-size: 0.75rem;
        margin-top: 0.1875rem;
      }
    }
  }
  .two {
    background: url("~assets/b2.png") no-repeat;
    background-size: 100%;
    margin-top: 1.25rem;
    .hui-left {
      p {
        color: #3a80ba;
      }
    }
    .hui-right {
      button {
        background: linear-gradient(270deg, #348aff, #6accff);
      }
      p {
        color: #40a2f4;
        font-size: 0.75rem;
      }
    }
  }
}
.con {
  h3 {
    color: #121212;
    font-size: 1.0625rem;
    margin-top: 1rem;
    margin-bottom: 1.25rem;
    padding: 0 4%;
  }
  .li {
    display: flex;
    margin: 0 4%;
    padding-bottom: 0.875rem;
    border-bottom: 0.03125rem solid #f3f3f3;
    margin-bottom: 0.9375rem;
    .left {
      width: 6.875rem;
      height: 5rem;
      margin-right: 0.75rem;
      background-color: #f5f5f5;
      text-align: center;
      img {
        height: 100%;
        max-width: 100%;
      }
    }
    .right {
      flex: 1;
      h4 {
        color: #323333;
        font-size: 1rem;
        font-weight: 400;
        margin-bottom: 0.1475rem;
        position: relative;
        top: -0.3125rem;
        span {
          float: right;
          padding: 0rem 0.375rem 0.1rem 0.375rem;
          border-radius: 0.125rem;
          background-color: #d1a23d;
          color: #fff;
          font-size: 0.6875rem;
        }
      }
      p {
        color: #7d7f80;
        font-size: 0.75rem;
        margin-bottom: 0.125rem;
        span {
          color: #333334;
          font-size: 0.8125rem;
        }
      }
      .total {
        span {
          color: #fe5830;
          font-size: 0.625rem;
          i {
            font-style: normal;
            font-size: 1rem;
          }
        }
      }
    }
  }
  a:last-child .li {
    border: 0;
    margin-bottom: 0;
  }
}
.other {
  padding: 0 4%;
  h3 {
    color: rgba(50, 51, 51, 1);
    font-size: 1rem;
    margin-bottom: 1.25rem;
    padding-top: 1rem;
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
        margin-bottom: 0.3rem;
        span {
          color: #b68826;
          font-size: 0.6875rem;
          float: right;
          padding: 0.15625rem 0.375rem 0.1875rem 0.375rem;
          background-color: #f8efdc;
          border-radius: 0.125rem;
          font-weight: 400;
        }
      }
      .pro-price {
        color: #7a7a7a;
        font-size: 0.75rem;
        margin-bottom: 0.1875rem;
        span {
          color: #ff5454;
          font-size: 1rem;
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
        margin-bottom: 0.3rem;
      }
      .pro-icon {
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
}
.nav {
  position: fixed;
  width: 92%;
  height: 3.75rem;
  background-color: #fff;
  padding: 0 4%;
  bottom: 0;
  display: flex;
  z-index: 1000;
  align-items: center;
  .nav-peo {
    position: relative;
    margin-right: 1.25rem;
    margin-left: 0.625rem;
    text-align: center;
    img {
      width: 1.5rem;
    }
    span {
      display: block;
      width: 0.8125rem;
      height: 0.8125rem;
      border-radius: 50%;
      text-align: center;
      line-height: 0.8125rem;
      font-size: 0.625rem;
      color: #fff;
      position: absolute;
      top: 0;
      right: 0;
      background-color: #f34f4f;
    }
    p {
      color: #626466;
      font-size: 0.75rem;
    }
  }
  .nav-msg {
    margin-right: 1.25rem;
    h5 {
      color: #222324;
      font-size: 1rem;
      font-weight: bold;
      margin-bottom: 0.25rem;
    }
    p {
      color: #696a6d;
      font-size: 0.75rem;
    }
  }
  button {
    width: 8rem;
    height: 2.5rem;
    border-radius: 0.25rem;
    background: linear-gradient(270deg, #348aff, #6accff);
    text-align: center;
    line-height: 2.5rem;
    color: #fff;
    font-size: 0.9375rem;
    border: 0;
    img {
      width: 0.9375rem;
      margin-bottom: -0.125rem;
      margin-right: 0.25rem;
    }
  }
  .nav-tel {
    margin-left: 0.625rem;
    color: #fff;
    background: linear-gradient(270deg, #b68826, #3fd6a6);
  }
}
.huo-msg .msg-con {
  width: 74vw;
  border-radius: 0.75rem;
  padding: 1.5625rem;
  background-color: #fff;
  z-index: 300;
}
.huo-msg .msg-con div {
  max-height: 280px;
  overflow-x: auto;
}
.huo-msg h4 {
  color: #2f3133;
  font-size: 1.25rem;
  text-align: center;
  margin-bottom: 1.25rem;
  font-weight: bold;
}
.huo-msg p {
  color: #626466;
  font-size: 0.8125rem;
  line-height: 1.1875rem;
  margin-bottom: 0.625rem;
}
.huo-msg p span {
  font-weight: bold;
}
.huo-msg img {
  width: 1rem;
  position: absolute;
  top: 1rem;
  right: 1rem;
}
</style>