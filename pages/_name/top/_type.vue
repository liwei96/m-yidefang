<template>
  <div id="TOPAll">
      <top-view :jkl="jkl"></top-view>
    <div class="topimg">
      <img src="~/assets/top-top.jpg" alt="" class="bg" />
      <div class="city" @click="gocity">{{cityname}}<img src="~/assets/top-down.png" alt="" /></div>
    </div>
    <div class="nav">
      <p :class="num == 0 ? 'active' : ''" @click="setnum(0)">刚需楼盘<span></span></p>
      <p :class="num == 1 ? 'active' : ''" @click="setnum(1)">投资地产<span></span></p>
      <p :class="num == 2 ? 'active' : ''" @click="setnum(2)">改善地产<span></span></p>
      <p :class="num == 3 ? 'active' : ''" @click="setnum(3)">现房<span></span></p>
    </div>
    <div class="topsbox" v-if="num==0">
      <div class="tops" v-for="(top,key) of top1s" :key="top.id">
          <nuxt-link :to="'/'+jkl+'/content/'+top.id">
        <div class="box">
          <div class="left">
            <img :src="top.img" alt="" />
          </div>
          <div class="right">
            <h4>{{top.name}}<span>{{top.status}}</span></h4>
            <p class="pri"><span>{{top.single_price}}</span>元/m²</p>
            <p class="types">{{top.type}} | {{top.city}}-{{top.country}} | {{parseInt(top.area_min)}}
                  <span v-if="top.area_max">-</span>
                  {{parseInt(top.area_max)}}m²</p>
            <p class="icons">
              <span class="one">{{top.decorate}}</span>
              <span v-if="top.railway">{{top.railway}}</span>
                <span>{{top.type}}</span>
            </p>
          </div>
        </div>
        </nuxt-link>
        <div class="bom">
          <img src="~/assets/top-bei.png" alt="" />
          人气榜第{{key+1}}名
          <button @click="pop('查询最底价', 34, '楼盘榜页+查询最底价',top.id)">查底价</button>
        </div>
      </div>
    </div>
    <div class="topsbox" v-if="num==1">
      <div class="tops" v-for="(top,key) of top2s" :key="top.id">
          <nuxt-link :to="'/'+jkl+'/content/'+top.id">
        <div class="box">
          <div class="left">
            <img :src="top.img" alt="" />
          </div>
          <div class="right">
            <h4>{{top.name}}<span>{{top.status}}</span></h4>
            <p class="pri"><span>{{top.single_price}}</span>元/m²</p>
            <p class="types">{{top.type}} | {{top.city}}-{{top.country}} | {{parseInt(top.area_min)}}
                  <span v-if="top.area_max">-</span>
                  {{parseInt(top.area_max)}}m²</p>
            <p class="icons">
              <span class="one">{{top.decorate}}</span>
              <span v-if="top.railway">{{top.railway}}</span>
                <span>{{top.type}}</span>
            </p>
          </div>
        </div>
          </nuxt-link>
        <div class="bom">
          <img src="~/assets/top-bei.png" alt="" />
          人气榜第{{key+1}}名
          <button @click="pop('查询最底价', 34, '楼盘榜页+查询最底价',top.id)">查底价</button>
        </div>
      </div>
    </div>
    <div class="topsbox" v-if="num==2">
      <div class="tops" v-for="(top,key) of top3s" :key="top.id">
          <nuxt-link :to="'/'+jkl+'/content/'+top.id">
        <div class="box">
          <div class="left">
            <img :src="top.img" alt="" />
          </div>
          <div class="right">
            <h4>{{top.name}}<span>{{top.status}}</span></h4>
            <p class="pri"><span>{{top.single_price}}</span>元/m²</p>
            <p class="types">{{top.type}} | {{top.city}}-{{top.country}} | {{parseInt(top.area_min)}}
                  <span v-if="top.area_max">-</span>
                  {{parseInt(top.area_max)}}m²</p>
            <p class="icons">
              <span class="one">{{top.decorate}}</span>
              <span v-if="top.railway">{{top.railway}}</span>
                <span>{{top.type}}</span>
            </p>
          </div>
        </div>
          </nuxt-link>
        <div class="bom">
          <img src="~/assets/top-bei.png" alt="" />
          人气榜第{{key+1}}名
          <button @click="pop('查询最底价', 34, '楼盘榜页+查询最底价',top.id)">查底价</button>
        </div>
      </div>
    </div>
    <div class="topsbox" v-if="num==3">
      <div class="tops" v-for="(top,key) of top4s" :key="top.id">
          <nuxt-link :to="'/'+jkl+'/content/'+top.id">
        <div class="box">
          <div class="left">
            <img :src="top.img" alt="" />
          </div>
          <div class="right">
            <h4>{{top.name}}<span>{{top.status}}</span></h4>
            <p class="pri"><span>{{top.single_price}}</span>元/m²</p>
            <p class="types">{{top.type}} | {{top.city}}-{{top.country}} | {{parseInt(top.area_min)}}
                  <span v-if="top.area_max">-</span>
                  {{parseInt(top.area_max)}}m²</p>
            <p class="icons">
              <span class="one">{{top.decorate}}</span>
              <span v-if="top.railway">{{top.railway}}</span>
                <span>{{top.type}}</span>
            </p>
          </div>
        </div>
          </nuxt-link>
        <div class="bom">
          <img src="~/assets/top-bei.png" alt="" />
          人气榜第{{key+1}}名
          <button @click="pop('查询最底价', 34, '楼盘榜页+查询最底价',top.id)">查底价</button>
        </div>
      </div>
    </div>
    <div class="line"></div>
    <div class="other">
      <h3>猜你喜欢</h3>
      <template>
        <nuxt-link :to="'/' + jkl + '/content/' + item.id" v-for="item of lists" :key="item.id">
          <div class="pro">
            <img :src="item.img" alt="" title="" />
            <div class="pro-msg">
              <h5>
                {{item.name}}
                <span>{{item.status}}</span>
              </h5>
              <p class="pro-price">
                <span>{{item.single_price}}</span>
                <i>元/m²</i>
              </p>
              <p class="attr">
                {{item.type}} | {{item.city}}-{{item.country}}
                | {{parseInt(item.area_min)}}
                  <span v-if="item.area_max">-</span>
                  {{parseInt(item.area_max)}}m²
              </p>
              <p class="pro-icon">
                <span class="pro-icon-zhuang">{{item.decorate}}</span>
                <span v-if="item.railway" class="pro-icon-type">{{item.railway}}</span>
              </p>
            </div>
          </div>
        </nuxt-link>
      </template>
    </div>
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
        :proname="proname"
      ></tan-view>
    </van-popup>
  </div>
</template>
<script>
import topView from "@/components/header.vue";
import tan from "@/components/tan.vue";
export default {
     components: {
    "top-view": topView,
    "tan-view": tan,
  },
    async asyncData (context) {
      try {

      }catch(err){
        
      }
    let ip=context.store.state.cookie.ip;
    let city = context.store.state.city;
    let token=context.store.state.cookie.token;
     let kk = context.params.type;
     let jkl=context.store.state.cookie.pinyin;
     let kid = context.store.state.cookie.kid ? context.store.state.cookie.kid : ''
    let other = context.store.state.cookie.other ? context.store.state.cookie.other : ''
    let [res]= await Promise.all([
      context.$axios.post('/api/first/feature_second_mobile',{ city: city, ip: ip, platform: 2, token: token,kid:kid,other:other })
      .then((resp)=>{
        let data = resp.data.data;
          for (let item of data.rigid_demand) {
            if (item.railway) {
              item.railway = item.railway.split(",")[0];
            }
          }
          for (let item of data.invest) {
            if (item.railway) {
              item.railway = item.railway.split(",")[0];
            }
          }
          for (let item of data.improve) {
            if (item.railway) {
              item.railway = item.railway.split(",")[0];
            }
          }
          for (let item of data.existing) {
            if (item.railway) {
              item.railway = item.railway.split(",")[0];
            }
          }
          for (let item of data.likes) {
            if (item.railway) {
              item.railway = item.railway.split(",")[0];
            }
          }
          return data;
      })
    ])
    return{
          top1s : res.rigid_demand,
          top2s : res.invest,
          top3s : res.improve,
          top4s : res.existing,
          lists : res.likes,
          jkl:jkl,
          title:res.title,
          description:res.description,
          keywords:res.keywords,
          num: kk
    }
  },
  head() {
    return {
      title: this.title || '易得房-楼盘榜',
      meta: [
        {
          name: "description",
          content: this.description || '易得房'
        },
        {
          name: "Keywords",
          content: this.keywords || '易得房'
        }
      ]
    };
  },
  data() {
    return {
      type: 0,
      cityname:'',
      tan: false,
      typenum: 0,
      typebtn: 1,
      name: "",
      remark: "",
      id: 0,
      proname:''
    };
  },
  methods:{
    pop(name, position, txt, id, proname) {
      console.log(88)
      this.name = name;
      this.typebtn = 1;
      this.typenum = position;
      this.tan = true;
      this.remark = txt;
      this.id = String(id);
      this.proname = proname;
    },
    cli(e) {
      this.tan = e;
    },
      setnum(n) {
          this.num = n
      },
      gocity() {
          this.$router.push("/" + this.jkl + "/address");
      },
      getmore() {
          var scrollTop = window.scrollY;
          console.log(scrollTop)
            if (scrollTop >= 100) {
                $(".nav").css({ position: "fixed", top: "44px" });
                $(".topsbox").css({ marginTop: "44px" });
            } else {
                $(".nav").css({ position: "relative", top: "0" });
                $(".topsbox").css({ marginTop: "0" });
            }
      }
  },
  mounted(){
      window.addEventListener("scroll", this.getmore);
      this.cityname = localStorage.getItem('cityname')
  },
  beforeDestroy() {
    window.removeEventListener("scroll", this.getmore);
  }
};
</script>
<style lang="less" scoped>
#TOPAll {
  padding-top: 2.75rem;
}
.topimg {
  position: relative;
  height: 6.25rem;
  .bg {
    width: 100%;
  }
  .city {
    position: absolute;
    width: 3.25rem;
    height: 1.1875rem;
    border-radius: 0.625rem;
    background: rgba(0, 0, 0, 0.2);
    top: 0.75rem;
    left: 0.9375rem;
    display: flex;
    align-items: center;
    justify-content: center;
    color: #ffffff;
    font-size: 0.75rem;
    img {
      width: 0.5rem;
      margin-left: 0.1875rem;
    }
  }
}
.nav {
  display: flex;
  align-items: center;
  justify-content: space-around;
  height: 2.75rem;
  width: 100%;
  background-color: #fff;
  z-index: 10;
  p {
    color: #626466;
    font-size: 0.9375rem;
  }
  .active {
    color: #3eacf0;
    font-size: 1rem;
    font-weight: bold;
    position: relative;
    span {
      position: absolute;
      display: block;
      background-color: #3eacf0;
      width: 1.5625rem;
      height: 0.15625rem;
      border-radius: 0.09375rem;
      left: 50%;
      margin-left: -0.78125rem;
      bottom: -0.75rem;
    }
  }
}
.topsbox {
    padding-top: .9375rem;
    padding-bottom: .625rem;
  .tops {
    margin: 0 0.9375rem;
    padding: 0.75rem 0.9375rem 1.125rem 0.75rem;
    border-radius: 0.75rem;
    box-shadow: 0px 0px 0.59375rem 0.03125rem rgba(0, 0, 0, 0.03);
    margin-bottom: 0.9375rem;
    .box {
      display: flex;
      margin-bottom: 1rem;
      .left {
        margin-right: 0.75rem;
        img {
          width: 6.875rem;
          height: 5rem;
          border-radius: 0.375rem;
        }
      }
      .right {
        margin-left: auto;
        flex: 1;
        h4 {
          color: #303233;
          font-size: 1rem;
          position: relative;
          top: -4px;
          span {
            padding: 0.1875rem 0.375rem;
            border-radius: 0.1875rem;
            float: right;
            font-size: 0.6875rem;
            background-color: #F8EFDC;
            color: #B68826;
          }
        }
        .pri {
          color: #ff6040;
          font-size: 0.8125rem;
          font-weight: bold;
          margin-bottom: 0.1rem;
          span {
            font-size: 1rem;
          }
        }
        .types {
          color: #646566;
          font-size: 0.75rem;
          margin-bottom: 0.3rem;
        }
        .icons {
          span {
            color: #7d7d80;
            font-size: 0.75rem;
            padding: 0.1875rem 0.3125rem;
            border-radius: 0.1875rem;
            background-color: #f5f5f5;
            margin-right: 0.375rem;
          }
          .one {
            color: #3eacf0;
            background-color: #ebf8ff;
          }
        }
      }
    }
    .bom {
      display: flex;
      align-items: center;
      color: #d58930;
      font-size: 0.75rem;
      img {
        width: 0.75rem;
        height: 0.75rem;
        margin-right: 0.1875rem;
      }
      button {
        width: 4.375rem;
        height: 1.5rem;
        border-radius: 0.75rem;
        text-align: center;
        line-height: 1.5rem;
        background-color: #ff6040;
        margin-left: auto;
        color: #fff;
        font-size: 0.75rem;
        border: 0;
        outline: none;
      }
    }
  }
}
.line {
    height: .625rem;
    background-color: #F7F7F7;
}
.other {
  padding: 1.25rem 4% 0 4%;
  border-radius: .75rem;
  background-color: #fff;
  h3 {
    color: #1f1f1f;
    font-size: 1.0625rem;
    margin-bottom: 1.125rem;
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
      border-radius: .375rem;
    }
    .pro-msg {
      flex: 1;
      h5 {
        color: #303233;
        font-size: 1rem;
        font-weight: bold;
        margin-top: -0.25rem;
        margin-bottom: 0.125rem;
        span {
          color: #B68826;
          font-size: 0.6875rem;
          float: right;
          padding: 0.1875rem 0.375rem;
          background-color: #F8EFDC;
          border-radius: 0.125rem;
          font-weight: 400;
        }
      }
      .pro-price {
        color: #7a7a7a;
        font-size: 0.75rem;
        margin-bottom: 0.1875rem;
        span {
          color: #FF6040;
          font-size: 1rem;
          font-weight: bold;
        }
        i {
          font-style: normal;
          color: #FF6040;
        }
      }
      .attr {
        color: #646466;
        font-size: 0.75rem;
        margin-bottom: 0.1875rem;
      }
      .pro-icon {
        .pro-icon-zhuang {
          color: #3EACF0;
          font-size: 0.6875rem;
          padding: 0.1875rem 0.375rem;
          background-color: #EBF8FF;
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
</style>
