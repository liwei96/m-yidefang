<template>
  <div id="content">
    <top-view :jkl="jkl" :totalnum="totalnum"></top-view>
    <div class="topnavlist" v-if="navtype">
      <p :class="nav == 0 ? 'active' : ''" @click="gonav(0)">
        户型<span></span>
      </p>
      <p :class="nav == 1 ? 'active' : ''" @click="gonav(1)">
        动态<span></span>
      </p>
      <p :class="nav == 2 ? 'active' : ''" @click="gonav(2)">
        分析<span></span>
      </p>
      <p :class="nav == 3 ? 'active' : ''" @click="gonav(3)">
        配套<span></span>
      </p>
    </div>
    <div class="topimg">
      <div class="swiper-topimg" @click="goimg">
        <div class="swiper-wrapper">
          <div class="swiper-slide" v-for="(item, key) in imgs" :key="key">
            <img
              :src="item.smallImg"
              :alt="abstract.name"
              :title="abstract.name"
            />
          </div>
        </div>
      </div>
      <!-- <img class="img-top" :src="basic.img" alt /> -->
      <!-- <span class="imgnum">共{{basic.img_num}}张</span> -->
      <!-- <div class="zhe" @click="goimg"></div> -->
      <p class="imgnum">共{{ imgnum }}张</p>
      <img
        src="~/assets/content-heart.png"
        alt=""
        class="top-heard"
        v-if="collect == 0"
        @click="shou"
      />
      <img
        src="~/assets/content-hearted.png"
        alt=""
        class="top-heard"
        v-if="collect == 1"
        @click="shou"
      />
      <div class="taps">
        <p
          v-for="(item, key) in imgmsg"
          :key="key"
          :class="imgmsgnum == key ? 'active' : ''"
          @click="setimgmsgnum(key)"
        >
          {{ item }}
        </p>
      </div>
    </div>
    <div class="intro">
      <div class="intro-top">
        <div class="top-left">
          <h2>{{ abstract.name }}</h2>
          <p>
            <span class="active">{{ abstract.sale_state }}</span>
            <span>{{ abstract.type }}</span>
            <span v-if="abstract.railways.length">{{
              abstract.railways[0]
            }}</span>
            <span v-else>{{abstract.features[0]}}</span>
          </p>
        </div>
        <div class="top-right" @click="pk">
          <img src="~/assets/content-pk.gif" alt="" />
          加入对比
        </div>
      </div>
      <div class="intro-fx" @click="pop('预约咨询', 119, '详情页+预约咨询')">
        <p>预约咨询<img src="~/assets/content-more.png" alt="" /></p>
      </div>
      <div class="intro-con">
        <nuxt-link :to="'/' + jkl + '/detail/' + id">
          <div class="inmore">
            详细信息<img src="~/assets/content-detail.png" alt="" />
          </div>
        </nuxt-link>
        <p>
          均价:
          <span class="price"> {{ abstract.single_price }}<i>元/m²</i> </span>
        </p>
        <p>
          开盘:
          <span>{{ abstract.open_time }}</span>
        </p>
        <p class="address" @click="gorim">
          地址:
          <span>{{ abstract.address }}</span>
          <img src="~/assets/map.png" alt />
        </p>
      </div>
      <p class="zhu">注：以上价格为开发商报价，最新价格以实际为准</p>
      <div class="btn">
        <button @click="pop('查询最底价', 34, '详情页+查询最底价')">
          <img src="~/assets/content-search.png" alt="" />
          查询最底价
        </button>
        <button @click="pop('变价通知我', 60, '详情页+变价通知我')">
          <img src="~/assets/content-change.png" alt="" />
          变价通知我
        </button>
      </div>
    </div>
    <div class="tel">
      <a :href="'tel:' + phone">
        <img class="telimg" src="~/assets/tel.jpg" alt />
        <p v-html="phonemsg"></p>
        <button><img src="~/assets/content-tel.png" />一键拨打</button>
      </a>
    </div>
    <div class="hui">
      <h3>
        优惠信息
        <img src="~/assets/ques.png" alt @click="huo = true" v-if="!participate"/>
        <img src="~/assets/ques.png" alt @click="huo1 = true" v-if="participate"/>
      </h3>
      <div class="hui-con" v-if="!participate">
        <div class="hui-left">
          <p>
            售楼处专供易得房客户<span>（{{ time }}截止）</span>
          </p>
        </div>
        <div class="hui-right">
          <button @click="pop('领取优惠', 94, '详情页+领取优惠')">
            领取优惠
          </button>
          <p>
            <span>{{ num1 }}人</span>已领取
          </p>
        </div>
      </div>
      <div class="phone-huo" v-if="participate">
        <img src="~/assets/phone-huo.jpg" alt="">
        <p>{{participate}}人已领</p>
        <button @click="pop('易得房专享购房送手机', 121, '详情页+易得房专享购房送手机')">立即抢</button>
      </div>
      <div class="hui-con two">
        <div class="hui-left">
          <p>
            免费专车1对1服务限时劵<span
              >（剩余{{ num2 }}张）</span
            >
          </p>
        </div>
        <div class="hui-right">
          <button @click="pop('免费专车看房', 86, '详情页+免费专车看房')">
            免费领取
          </button>
          <p>
            <span>{{ num3 }}人</span>已领取
          </p>
        </div>
      </div>
    </div>
    <div class="te" v-if="specials.rooms">
      <h3>
        <img src="~/assets/index-hot.jpg" alt="" />
        今日特价房
        <span>{{ begintime }}更新</span>
        <!-- <img src="~/assets/tit.png" alt class="te-tit" /> -->
      </h3>
      <ul class="tabs">
        <li v-for="(item, key) in specials.rooms" :key="key">
          <div class="left">
            <p class="msg">
              房号-{{
                item.number.split("-")[item.number.split("-").length - 1]
              }}&nbsp;&nbsp; 面积-{{ parseInt(item.area) }}m²
            </p>
            <div class="pri">
              <p class="tepri">
                特价<span>{{ (parseInt(item.total) / 10000).toFixed(1) }}</span
                >万
              </p>
              <p class="oldpri">原价<span>{{ (parseInt(item.original_total) / 10000).toFixed(1) }}</span>万</p>
            </div>
          </div>
          <p class="right" @click="pop('抢特价房', 114, '详情页+抢特价房')">
            抢特价房
          </p>
        </li>
      </ul>
      <div class="zhe" @click="showmores" v-if="morebtns">
        <img src="~/assets/huo-down.png" alt />
      </div>
      <div class="bom">
        <notice-bar
          color="#807D7D"
          background="rgba(0,0,0,0)"
          left-icon="volume-o"
          :text="specials.content"
        />
        <button @click="pop('咨询特价房', 93, '详情页+咨询特价房')">
          咨询特价房
        </button>
      </div>
    </div>
    <div class="hus" v-show="house_types.length">
      <h5>
        主力户型
        <nuxt-link :to="'/' + jkl + '/hus/' + id">
          <span>
            全部户型
            <img src="~/assets/home-more.png" alt />
          </span>
        </nuxt-link>
      </h5>
      <div class="swiper-hu">
        <div class="swiper-wrapper">
          <div
            class="swiper-slide"
            v-for="(item, key) in house_types"
            :key="key"
          >
            <nuxt-link :to="'/' + jkl + '/hu/'+abstract.id+'/' + item.id">
              <div class="hu-top">
                <img
                  :src="item.small"
                  :alt="abstract.name + item.title + '户型图'"
                  :title="abstract.name + item.title + '户型图'"
                />
              </div>
            </nuxt-link>
            <div class="hu-bom">
              <p class="name">
                {{ item.title }}
                <span>{{ item.state }}</span>
              </p>
              <p class="type">
                <span class="t1">建面 {{ item.area }}m²</span>
                <!-- <span>类型 {{ item.type }}</span> -->
              </p>
              <p class="huprice">
                约<span>{{ item.price }}</span
                >万起
              </p>
            </div>
          </div>
        </div>
      </div>
      <button @click="pop('领取全部户型资料', 56, '详情页+领取全部户型资料')">
        领取全部户型资料
      </button>
    </div>
    <div class="dynamic">
      <h3>
        最新动态
        <img src="~/assets/content-tt.png" alt="" class="tt" />
        <nuxt-link :to="'/' + jkl + '/promsg/' + id + '/0'">
          <span>
            全部动态
            <img src="~/assets/j-more.png" alt />
          </span>
        </nuxt-link>
      </h3>
      <ul>
        <li v-for="(item, key) in dynamics" :key="key">
          <p>{{ item.content }}</p>
          <span>{{ item.time }}</span>
          <div class="dynamicimgs">
            <img
              :src="allimgs[key].smallImg"
              :alt="abstract.name + '实时动态'"
              :title="abstract.name + '实时动态'"
            />
          </div>
        </li>
      </ul>
      <div class="btn">
        <input
          type="text"
          placeholder="输入手机号订阅最新动态"
          v-model="dongtel"
        />
        <button @click="apply">立即订阅</button>
      </div>
    </div>
    <div class="newprice" v-show="deal_prices.length">
      <h3 v-show="deal_prices.length">
        最新成交价
        <p>6小时前更新</p>
      </h3>
      <div
        id="chart"
        style="width: 100%; height: 180px"
        v-show="deal_prices.length"
      ></div>
      <p v-show="deal_prices.length">2021年</p>
      <div class="tab" v-show="deal_prices.length">
        <table cellspacing="0" cellpadding="0">
          <tbody>
            <tr class="thead">
              <th>日期</th>
              <th>成交套数</th>
              <th>成交金额</th>
            </tr>
            <tr v-for="(item, key) in deal_prices" :key="key">
              <td>{{ item.time }}</td>
              <td>{{ item.num }}套</td>
              <td>***万</td>
            </tr>
          </tbody>
        </table>
        <div class="zhe" @click="showmore" v-if="morebtn">
          <img src="~/assets/huo-down.png" alt />
        </div>
      </div>
      <div>
        <notice-bar
          color="#807D7D"
          background="rgba(0,0,0,0)"
          left-icon="volume-o"
          :scrollable="false"
        >
          <van-swipe
            vertical
            class="notice-swipe"
            :autoplay="3000"
            :show-indicators="false"
          >
            <van-swipe-item v-for="(item, key) in deal_prices" :key="key"
              >客户 {{ item.tel }}
              {{ item.time.substr(5) }} 成功购房</van-swipe-item
            >
          </van-swipe>
        </notice-bar>
      </div>
      <button
        @click="pop('查询最新成交价', 81, '详情页+查询最新成交价')"
        v-show="deal_prices.length"
      >
        查询最新成交价
      </button>
    </div>
    <div class="ziliao">
      <h3>楼盘分析资料</h3>
      <div class="tab">
        <p :class="tabnum == 1 ? 'active' : ''" @click="tabnum = 1">投资分析</p>
        <p @click="tabnum = 2" :class="tabnum == 2 ? 'active' : ''">宜居分析</p>
      </div>
      <div class="liao-msg" v-if="tabnum == 1">
        <template v-for="(item, key) in analysis.investment">
          <p :key="key" >{{ item }}</p>
        </template>
      </div>
      <div class="liao-msg" v-if="tabnum == 2">
        <template v-for="(item, key) in analysis.livable">
          <p :key="key" >{{ item }}</p>
        </template>
      </div>
      <div
        class="zhe"
        @click="pop('领取完整分析报告', 96, '详情页+领取完整分析报告')"
      >
        <img src="~/assets/content-suo.png" alt="" />
      </div>
      <button @click="pop('领取完整分析报告', 96, '详情页+领取完整分析报告')">
        领取完整分析报告
      </button>
    </div>
    <div class="zixun">
      <h3>
        新房咨询师<span @click="gojoin"
          >申请入驻<img src="~/assets/content-join.png" alt=""
        /></span>
      </h3>
      <p class="xun-icon">
        <span> <img src="~/assets/save.png" alt />专业服务 </span>
        <span> <img src="~/assets/icon-path.png" alt />区域解读 </span>
        <span> <img src="~/assets/icon-pin.png" alt />户型分析 </span>
      </p>
      <div class="peo" v-for="(item, key) in staffs" :key="key">
        <img class="peoimg" :src="item.image" alt />
        <div class="peo-msg">
          <h6>
            {{ item.name }}
            <span>满意度5分</span>
          </h6>
          <p v-if="key == 0">了解房源特色，专业挑好房</p>
          <p v-if="key == 1">为客户提供专业的购房建议</p>
        </div>
        <img
          class="peoicon"
          src="~/assets/message.png"
          alt
          @click="pop('咨询服务', 48, '详情页+咨询服务')"
        />
        <a :href="'tel:' + phone">
          <img class="peoicon" src="~/assets/tel.png" alt />
        </a>
      </div>
    </div>
    <div class="zhou">
      <h4>
        周边配套
        <!-- <nuxt-link :to="'/' + jkl + '/rim/' + id">
          <span>
            详细配套
            <img src="~/assets/go.png" alt />
          </span>
        </nuxt-link> -->
      </h4>
      <p class="txt">位置：<span>{{abstract.address}}</span></p>
      <p class="txt">
        配套：<span
          class="talk"
          @click="
            pop('获取周边5公里详细配套', 69, '详情页+获取周边5公里详细配套')
          "
          >咨询具体位置和周边设施情况</span
        ><img
          src="~/assets/content-map-talk.png"
          alt=""
          @click="
            pop('获取周边5公里详细配套', 69, '详情页+获取周边5公里详细配套')
          "
        />
      </p>
      <div class="swiper-map">
        <div class="swiper-wrapper">
          <div class="swiper-slide">
            <div
              :class="mapnum === 1 ? 'tegood active' : 'tegood'"
              @click="setmap(1, '公交')"
            >
              公交
              <i></i>
            </div>
          </div>
          <div class="swiper-slide">
            <div
              :class="mapnum === 0 ? 'tegood active' : 'tegood'"
              @click="setmap(0, '地铁')"
            >
              地铁
              <i></i>
            </div>
          </div>
          <div class="swiper-slide">
            <div
              :class="mapnum === 2 ? 'tegood active' : 'tegood'"
              @click="setmap(2, '教育')"
            >
              教育
              <i></i>
            </div>
          </div>
          <div class="swiper-slide">
            <div
              :class="mapnum === 3 ? 'tegood active' : 'tegood'"
              @click="setmap(3, '医院')"
            >
              医院
              <i></i>
            </div>
          </div>
          <div class="swiper-slide">
            <div
              :class="mapnum === 4 ? 'tegood active' : 'tegood'"
              @click="setmap(4, '购物')"
            >
              购物
              <i></i>
            </div>
          </div>
          <div class="swiper-slide">
            <div
              :class="mapnum === 5 ? 'tegood active' : 'tegood'"
              @click="setmap(5, '美食')"
            >
              美食
              <i></i>
            </div>
          </div>
          <div class="swiper-slide">
            <div
              :class="mapnum === 6 ? 'tegood active' : 'tegood'"
              @click="setmap(6, '娱乐')"
            >
              娱乐
              <i></i>
            </div>
          </div>
        </div>
      </div>
      <div id="map" @click="gomap"></div>
      <div id="panel" style="display: none"></div>

      <div class="map-msg">
        <ul class="map-msg-con">
          <li>
            <h6>武林广场</h6>
            <p>
              地铁1号线
              <span> <img src="~/assets/dian.png" />800m </span>
            </p>
          </li>
          <li>
            <h6>武林广场</h6>
            <p>
              地铁1号线
              <span> <img src="~/assets/dian.png" />800m </span>
            </p>
          </li>
        </ul>
        <p class="map-tishi" v-if="isnull">
          附近没有{{ mapname }}，您可以看看其他信息
        </p>
      </div>
      <button
        class="btnicon"
        @click="
          pop('获取周边5公里详细配套', 69, '详情页+获取周边5公里详细配套')
        "
      >
        获取周边5公里详细配套
      </button>
    </div>
    <div class="dian">
      <h3>
        楼盘点评
        <nuxt-link :to="'/' + jkl + '/comments/' + id" v-if="comments.length">
          <span>
            更多点评
            <img src="~/assets/go.png" alt />
          </span>
        </nuxt-link>
      </h3>
      <ul v-if="comments.length">
        <li v-for="(item, key) in comments" :key="key">
          <div class="dian-top">
            <img src="~/assets/jiapeo.png" alt class="peo" />
            <div class="top-con">
              <h6>
                {{ item.mobile }}
                <span v-if="item.mine" @click="del(item.id, key)">删除</span>
              </h6>

              <p>{{ item.time }}</p>
            </div>
            <div
              :class="item.my_like == 1 ? 'top-right active' : 'top-right'"
              @click="like(item.id, key)"
            >
              <img :src="item.my_like == 1 ? img1 : img" alt />
              赞({{ item.like_count }})
            </div>
          </div>
          <nuxt-link :to="'/' + jkl + '/commentdetail/' + id + '/' + item.id">
            <div class="dian-bom">{{ item.content }}</div>
          </nuxt-link>
        </li>
      </ul>
      <p class="tishi" v-if="!comments.length">暂无点评，快来点评吧</p>
      <button @click="dianping">我要点评</button>
    </div>
    <div class="wen">
      <h3>
        楼盘问答
        <nuxt-link :to="'/' + jkl + '/questions/' + id" v-if="questions.length">
          <span>
            全部问答
            <img src="~/assets/go.png" alt />
          </span>
        </nuxt-link>
      </h3>
      <ul v-if="questions.length">
        <template v-for="(item, key) in questions">
          <nuxt-link :to="'/' + jkl + '/answer/' + item.id+'/'+abstract.id" :key="key">
            <li>
              <p class="con">
                <span>问</span>
                {{ item.question }}
              </p>
              <p class="num">共1个专业问答</p>
            </li>
          </nuxt-link>
        </template>
      </ul>
      <p class="tishi" v-if="!questions.length">暂无问答，快来提问吧</p>
      <button @click="tiwen">我要提问</button>
    </div>
    <div class="my-infos" v-if="infos.length > 0">
      <h4>
        相关资讯<nuxt-link :to="'/' + jkl + '/promsg/' + id + '/1'"
          ><span>更多资讯 <img src="~/assets/go.png" alt /></span
        ></nuxt-link>
      </h4>
      <ul>
        <nuxt-link
          v-for="(item, key) in infos"
          :key="item.id"
          :to="'/' + jkl + '/info/' + item.id"
        >
          <li v-if="key <= 2">{{ item.title }}</li>
        </nuxt-link>
      </ul>
    </div>
    <div class="other">
      <h3>为你推荐</h3>
      <template v-for="(item, key) in recommends">
        <nuxt-link :to="'/' + jkl + '/content/' + item.id" :key="key">
          <div class="pro">
            <img :src="item.image" :alt="item.name" :title="item.name" />
            <div class="pro-msg">
              <h5>
                {{ item.name }}
                <span>{{ item.state }}</span>
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
                <span class="pro-icon-zhuang">{{ item.decorate.substr(0,2) }}</span>
                <span class="pro-icon-type" v-if="item.railway">{{ item.railway }}</span>
                <span class="pro-icon-type" v-if="item.feature">{{ item.feature }}</span>
              </p>
            </div>
          </div>
        </nuxt-link>
      </template>
      <!-- <div class="pro" v-for="(item, key) in recommends" :key="key">
        <img :src="item.img" alt />
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
            {{ item.type }} | {{ item.city }}-{{ item.country }} |
            {{ item.area }}m²
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
      </div> -->
    </div>
    <nav-view
      :phone="phone"
      @fot="chang($event)"
      :totalnum="totalnum"
      :jkl="jkl"
    ></nav-view>
    <div class="imgbox" @click="srctype = false" v-show="srctype">
      <img :src="src" alt />
    </div>
    <div class="talkbox" v-if="talktype">
      <img
        src="~/assets/w-del.png"
        alt=""
        class="del"
        @click="talktype = false"
      />
      <div class="peo">
        <div class="left">
          <img :src="staffimg" alt="" />
        </div>
        <div class="right">
          <h3>{{ staffname }} <span>新房咨询</span></h3>
          <p>从业咨询服务6年</p>
        </div>
      </div>
      <div class="msg">
        <div class="li">
          <p class="num">
            <span>{{ usernum }}</span
            >人
          </p>
          <p class="txt">服务客户</p>
        </div>
        <div class="li">
          <p class="num">
            <span>{{ looknum }}</span
            >次
          </p>
          <p class="txt">带看客户</p>
        </div>
        <div class="li">
          <p class="num">
            <span>{{ rate }}</span
            >%
          </p>
          <p class="txt">好评率</p>
        </div>
      </div>
      <div class="btn">
        <button @click="talk">在线咨询</button>
        <button @click="talktype = false">稍后咨询</button>
      </div>
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
        :proname="abstract.name"
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
    <van-popup v-model="huo1" :style="{ background: 'rgba(0,0,0,0)' }">
      <div class="huo-msg">
        <div class="msg-con">
          <h4>活动规则</h4>
          <img @click="huo1 = false" src="~/assets/w-del.png" alt />
          <div>
            <p>
              即日起，凡是通过本线上营销中心成交的本项目，即送苹果12 pro max一台，平台合计1000台手机送完为止。具体活动详情来电咨询
            </p>
            <p>注：活动最终解释权归易得房所有</p>
          </div>
        </div>
      </div>
    </van-popup>
    <van-popup v-model="show" :style="{ background: 'rgba(0,0,0,0)' }">
      <hong
        @close="close"
        :id="id"
        :txt="remark"
        :name="name"
        :typebtn="typebtn"
        :typenum="typenum"
        :proname="abstract.name"
        :num="num"
      ></hong>
    </van-popup>
  </div>
</template>
<script>
import topView from "@/components/header.vue";
import tan from "@/components/tan.vue";
import nav from "@/components/nav.vue";
import { NoticeBar } from "vant";
import { delcomm, likecomm, collect, put } from "@/api/api";
import Swiper from "swiper";
import "swiper/css/swiper.min.css";
export default {
  components: {
    NoticeBar,
    "top-view": topView,
    "nav-view": nav,
    "tan-view": tan,
  },
  async asyncData(context) {
    try {
      let host = context.store.state.host;
      let id = context.params.id;
      let token = context.store.state.cookie.token || "";
      let jkl = context.params.name;
      let other = context.query.other;
      let userid = context.store.state.userid
      if (other) {
        context.store.state.other = other;
      } else {
        if (context.store.state.other) {
          other = context.store.state.other;
        }
      }
      let kid = context.query.kid;
      let [res1] = await Promise.all([
          context.$axios
          .get("/edefang_new_mobile/building", {
            params: {
              id: id,
              other: other,
              userId:userid
            },
          })
          .then((resp) => {
            let data = resp.data.data;
            data.prices = [];
            if (data.transactionPrices) {
              for (let val in data.transactionPrices) {
                data.prices[val] = [
                  data.transactionPrices[val].time.substr(5),
                  data.transactionPrices[val].price,
                ];
              }
            }else{
              data.transactionPrices = []
            }
            if (data.special_price_rooms) {
              data.begintime = data.special_price_rooms.rooms[0].begin
            }else{
              data.special_price_rooms = {
                rooms:false
              }
            }
            data.allimgs = data.image.effect.concat(data.image.example)
            // console.log(data)
            return data;
          }),
      ]);
      return {
        begintime:res1.begintime,
        effects: res1.image ? res1.image.effect : [],
        examples: res1.image ? res1.image.example : [],
        // traffics: res.img ? res.img.img.traffic : [],
        imgnum: res1.image ? res1.image.count : 0,
        abstract: res1.building,
        phone: res1.phone,
        // scores: res.scores,
        house_types: res1.house_type,
        dynamics: res1.dynamics,
        staffs: res1.staffs,
        analysis: res1.analysis,
        participate: res1.participate,
        deal_prices: res1.transactionPrices,
        prices: res1.prices,
        comments: res1.comments,
        questions: res1.questions,
        recommends: res1.recommends,
        jkl: jkl,
        id: id,
        specials: res1.special_price_rooms,
        // count: res.virtual_num,
        collect: res1.collect,
        cityname: res1.current_city.name,
        othercode: other,
        kidcode: kid,
        title: res1.header.title,
        description: res1.header.description,
        keywords: res1.header.keywords,
        host: host,
        infos: res1.articles,
        allimgs: res1.allimgs
      };
    } catch (err) {
      console.log("errConsole========:", err);
      context.error({ statusCode: 404, message: "页面未找到或无数据" });
    }
  },
  head() {
    return {
      title: this.title || this.abstract.name + "-" + this.cityname,
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
      allimgs: [],
      huo1: false,
      nav: 0,
      show: false,
      talktype: false,
      time: "7月24日",
      count: {},
      tan: false,
      typenum: 0,
      typebtn: 1,
      name: "",
      remark: "",
      recommends: [],
      questions: [],
      comments: [],
      deal_prices: [],
      analysis: [],
      staffs: [],
      dynamics: [],
      house_types: [],
      scores: {},
      phone: "",
      abstract: {},
      btn: false,
      mapnum: 1,
      tabnum: 1,
      isnull: false,
      morebtn: true,
      imgs: [],
      imgmsg: ["效果图", "样板房"],
      imgmsgnum: 0,
      morebtns: false,
      navs: ["优惠", "分析", "户型", "动态", "配套", "点评", "问答"],
      navnum: 0,
      src: require("~/assets/lun02.jpg"),
      srctype: false,
      mapname: "公交",
      effects: [],
      examples: [],
      traffics: [],
      imgnum: 0,
      id: 0,
      specials: {},
      hour: 0,
      huo: false,
      img: require("~/assets/noclick.png"),
      img1: require("~/assets/checked.png"),
      heart: require("~/assets/home-heart.png"),
      hearted: require("~/assets/collected.png"),
      phonemsg: "",
      kidcode: "",
      othercode: "",
      timer: "",
      lucktypes: true,
      ws: {},
      totalnum: 0,
      usernum: 0,
      looknum: 0,
      rate: 0,
      stafftel: 0,
      staffname: "",
      staffimg: "",
      txt: "家园",
      num: "",
      dongtel: "",
      navtype: false,
      num1: 221,
      num2: 256,
      num3: 209
    };
  },
  methods: {
    gonav(n) {
      switch (n) {
        case 0:
          let hu = $(".hus").offset().top;
          $("html,body").animate({ scrollTop: hu - 140 }, 300);
          break;
        case 1:
          let dynamic = $(".dynamic").offset().top;
          $("html,body").animate({ scrollTop: dynamic - 140 }, 300);
          break;
        case 2:
          let ziliao = $(".ziliao").offset().top;
          $("html,body").animate({ scrollTop: ziliao - 140 }, 300);
          break;
        case 3:
          let zhou = $(".zhou").offset().top;
          $("html,body").animate({ scrollTop: zhou - 140 }, 300);
          break;
      }
    },
    gomap() {
      this.$router.push("/" + this.jkl + "/rim/" + this.abstract.id);
    },
    gojoin() {
      window.location.href =
        "http://recruit.jy1980.com?type=1&city=" + localStorage.getItem("city");
    },
    setnav() {
      let that = this;
      var scrollTop = window.scrollY;
      let hu = $(".hus").offset().top;
      let dynamic = $(".dynamic").offset().top;
      let ziliao = $(".ziliao").offset().top;
      let zhou = $(".zhou").offset().top;
      if (scrollTop > 160) {
        this.navtype = true;
        if (scrollTop >= hu - 60) {
          this.nav = 0;
        }
        if (scrollTop >= dynamic - 160) {
          this.nav = 1;
        }
        if (scrollTop >= ziliao - 160) {
          this.nav = 2;
        }
        if (scrollTop >= zhou - 160) {
          this.nav = 3;
        }
      } else {
        this.navtype = false;
      }
    },
    apply() {
      var phone = this.dongtel;
      var pattern_phone = /^1[3-9][0-9]{9}$/;
      if (phone == "") {
        this.toast("手机号不能为空");
        return;
      } else if (!pattern_phone.test(phone)) {
        this.toast("手机号码不合法");
        return;
      }
      let id = this.id;
      let ip = ip_arr["ip"];
      let city = $cookies.get("city");
      let kid = $cookies.get("kid");
      let other = $cookies.get("other");
      let normal = {
        ip: ip,
        tel: phone,
        city: city,
        position: 63,
        page: 3,
        type: 9,
        project: id,
        kid: kid,
        other: other,
        platform: 2,
      };
      put(normal).then((res) => {
        if (res.data.code == 200) {
          this.toast(res.data.msg || res.data.message);
        }
        if (res.data.code == 500) {
          this.toast(res.data.msg || res.data.message);
        }
      });
    },
    sign(name, position, txt, num) {
      this.name = name;
      this.typebtn = 1;
      this.typenum = position;
      this.show = true;
      this.remark = txt;
      this.num = num;
    },
    close() {
      console.log(55);
      this.show = false;
    },
    handleScroll() {
      this.lucktypes = false;
      if (this.timer) {
        clearTimeout(this.timer);
      }
      let that = this;
      this.timer = setTimeout(() => {
        that.lucktypes = true;
      }, 800);
    },
    goluck() {
      this.$router.push("/" + this.jkl + "/backhome");
    },
    dianping() {
      let token = $cookies.get("token");
      if (token) {
        this.$router.push("/" + this.jkl + "/comment/" + this.id);
      } else {
        let url = "/" + this.jkl + "/comment/" + this.id;
        sessionStorage.setItem("path", url);
        this.$router.push("/" + this.jkl + "/login");
      }
    },
    tiwen() {
      let token = $cookies.get("token");
      if (token) {
        this.$router.push("/" + this.jkl + "/quiz/" + this.id);
      } else {
        let url = "/" + this.jkl + "/quiz/" + this.id;
        sessionStorage.setItem("path", url);
        this.$router.push("/" + this.jkl + "/login");
      }
    },
    del(id, key) {
      let token = $cookies.get("token");
      if (token) {
        delcomm({ token: token, id: id }).then((res) => {
          if (res.data.code == 200) {
            this.toast("删除成功");
            this.$router.go(0);
          } else {
            let url = this.$route.path;
            sessionStorage.setItem("path", url);
            this.$router.push("/" + this.jkl + "/login");
          }
        });
      } else {
        let url = this.$route.path;
        sessionStorage.setItem("path", url);
        this.$router.push("/" + this.jkl + "/login");
      }
    },
    like(id, key) {
      let token = $cookies.get("token");
      if (token) {
        likecomm({ token: token, id: id }).then((res) => {
          if (res.data.code == 200) {
            // this.$router.go(0);
            if (this.comments[key].my_like == 1) {
              this.comments[key].my_like = 0;
              this.comments[key].like_count = this.comments[key].like_count - 1;
            } else {
              this.comments[key].my_like = 1;
              this.comments[key].like_count = this.comments[key].like_count + 1;
            }
          } else {
            let url = this.$route.path;
            sessionStorage.setItem("path", url);
            this.$router.push("/" + this.jkl + "/login");
          }
        });
      } else {
        let url = this.$route.path;
        sessionStorage.setItem("path", url);
        this.$router.push("/" + this.jkl + "/login");
      }
    },
    setimgmsgnum(e) {
      this.imgmsgnum = e;
      if (e == 0) {
        this.imgs = this.effects;
      } else if (e == 1) {
        this.imgs = this.examples;
      } else {
        this.imgs = this.traffics;
      }
    },
    showmores() {
      this.morebtns = false;
      $(".tabs").css({ height: "auto" });
      $(".te").css("height", "auto");
    },
    showmore() {
      this.morebtn = false;
      $(".tab").css("height", "auto");
    },
    gorim() {
      this.$router.push("/" + this.jkl + "/rim/" + this.id);
    },
    mmap() {
      this.over = false;
      let that = this;
      let baidu = [that.abstract.longitude, that.abstract.latitude];
      let img = require("~/assets/mappro.png");
      let pro = that.abstract.name;
      let add = that.abstract.address;
      AMap.convertFrom(baidu, "baidu", function (status, result) {
        if (result.info === "ok") {
          var lnglats = result.locations; // Array.<LngLat>
          that.pois = [lnglats[0].lng, lnglats[0].lat];
          var map = new AMap.Map("map", {
            zoom: 14, //初始化地图层级
            center: that.pois, //初始化地图中心点
            zoomEnable: true,
            dragEnable: true,
          });
          let content = `<div
          style="width:140px;height: 36px;box-shadow:0px 0px 5px 0px rgba(6,0,1,0.1);border-radius:18px;padding-left: 17px;position: relative;background: #fff;z-index:10">
          <div style="float: left;width:100%">
            <h5 style="color: #121212;font-size: 12px;margin:0;margin-top: 3px;text-align:center">${pro}</h5>
            <p style="color: #919499;font-size: 10px;margin:0;overflow: hidden;text-overflow:ellipsis;white-space: nowrap;">${add}</p>
          </div>
          <div
            style="position: absolute;border:8px solid transparent;border-top-color: #fff;bottom:-16px;left:50%;margin-left: -8px;">
          </div>
        </div>`;
          let marker = new AMap.Marker({
            content: content,
            position: that.pois,
            offset: new AMap.Pixel(-78, -44),
          });
          let con =
            '<div style="width: 24px;height: 24px;border-radius: 50%;background:rgba(71,161,255,0.3);position: relative;"><div style="width: 6px;height: 6px;border-radius: 50%;background:rgba(71,161,255,1);position: absolute;top:50%;left:50%;margin-top: -3px;margin-left: -3px;"></div></div>';
          let mark = new AMap.Marker({
            content: con,
            position: that.pois,
            offset: new AMap.Pixel(-12, -12),
          });
          mark.setMap(map);
          marker.setMap(map);
          AMap.service(["AMap.PlaceSearch"], function () {
            // eslint-disable-line no-unused-vars
            //构造地点查询类
            var placeSearch = new AMap.PlaceSearch({
              pageSize: 10, // 单页显示结果条数
              pageIndex: 1, // 页码
              city: "", // 兴趣点城市
              citylimit: false, //是否强制限制在设置的城市内搜索
              map: map, // 展现结果的地图实例
              panel: "panel", // 结果列表将在此容器中进行展示。
              autoFitView: false, // 是否自动调整地图视野使绘制的 Marker点都处于视口的可见范围
            });
            var cpoint = that.pois;
            let name = that.mapname;
            console.log(name);
            placeSearch.searchNearBy(name, cpoint, 1200, function (
              // eslint-disable-line no-unused-vars
              status,
              result
            ) {
              var ht = "";
              // console.log(ht)
              let img = require("~/assets/dian.png");
              if (JSON.stringify(result) == "{}") {
                that.isnull = true;
                // that.setzhou(name, 0);
              } else {
                that.isnull = false;
                // that.setzhou(name, result.poiList.pois.length);
                $.each(result.poiList.pois, function (i, e) {
                  if (i <= 2) {
                    var p2 = [e.location.lng, e.location.lat];
                    var s = AMap.GeometryUtil.distance(cpoint, p2) / 1000;
                    s = s.toFixed(1);
                    ht += `
                    <li>
                      <h5>${e.name}</h5>
                      <div class="mapss"><p>${e.address}</p> <span><img src="${img}">${s}km</span></div>
                    </li>
                    `;
                  }
                });
              }
              $(".map-msg-con").html(ht);
            });
          });
        }
      });
    },
    setmap(id, name) {
      this.mapnum = id;
      this.mapname = name;
      this.mmap();
    },
    look(src) {
      this.src = src;
      this.srctype = true;
    },
    drawline() {
      // 初始化图表标签
      //下面这是vue中使用
      var myChart = this.$echarts.init(document.getElementById("chart"));
      let that = this;
      var options = {
        //定义一个标题
        legend: {
          data: ["AI"],
        },
        color: ["#F2C76A"],
        grid: {
          top: "10%",
          left: "8.2%",
          right: 0,
          bottom: "10%",
        },
        //X轴设置
        xAxis: [
          {
            type: "category",
            data: that.times,
            axisLine: {
              lineStyle: {
                color: "#EEEEEE",
              },
            },
            axisLabel: {
              show: true,
              textStyle: {
                color: "#969699",
              },
            },
          },
        ],
        yAxis: [
          {
            min: 0,
            // max: that.max,
            splitLine: {
              lineStyle: {
                type: "dashed",
                color: "#E4E4EB",
              },
            },
            axisLine: {
              lineStyle: {
                color: "#919499",
              },
              show: false,
            },
          },
        ],
        series: [
          {
            data: that.prices,
            // smooth: 0.6,
            // symbol: "circle",
            lineStyle: {
              color: "#F2C76A",
              width: 1,
            },
            // areaStyle: {},
            type: "bar",
            itemStyle: {
              // 转折点 控制
              color: "#F2C76A",
            },
            barWidth: 20, // 柱形的宽度
            barCategoryGap: "10%", // 柱形的间距
          },
        ],
      };
      myChart.setOption(options);
    },
    goimg() {
      this.$router.push("/" + this.jkl + "/photos/" + this.$route.params.id);
    },
    pk() {
      let arr = $cookies.get("ids");
      sessionStorage.setItem("pktype", 1);
      if (!arr) {
        arr = this.id;
        $cookies.set("ids", arr);
        this.$router.push("/" + this.jkl + "/pk/" + this.id);
      } else {
        let k = arr.split(",");
        k.unshift(this.id);
        let kk = [];
        for (let val of k) {
          if (kk.indexOf(String(val)) == -1) {
            kk.push(val);
          }
        }
        kk = kk.slice(0, 4);
        arr = kk.join(",");
        $cookies.set("ids", arr);
        this.$router.push("/" + this.jkl + "/pk/" + arr);
      }
    },
    pop(name, position, txt) {
      this.name = name;
      this.typebtn = 1;
      this.typenum = position;
      this.tan = true;
      this.remark = txt;
    },
    cli(e) {
      this.tan = e;
    },
    chang(data) {
      this.typenum = data.position;
      this.name = data.name;
      this.typebtn = 1;
      this.tan = true;
      this.remark = "详情页+预约看房";
    },
    shou() {
      if ($cookies.get("token")) {
        let that = this;
        collect({ id: that.id, type: 1, token: $cookies.get("token") }).then(
          (res) => {
            if (res.data.code == 200) {
              if (that.collect == 0) {
                that.toast("收藏成功");
                that.collect = 1;
              } else {
                that.toast("取消成功");
                that.collect = 0;
              }
            }
          }
        );
      } else {
        this.$router.push("/" + this.jkl + "/login");
      }
    },
    putcard() {
      let urlid = this.$route.params.id;
      let id = sessionStorage.getItem(urlid);
      let host = this.$store.state.hostname;
      let pp = {
        controller: "Staff",
        action: "info",
        params: { uuid: id, host: host },
      };
      if (id) {
        this.ws.send(JSON.stringify(pp));
      }
    },
    talk() {
      let urlid = this.$route.params.id;
      let id = sessionStorage.getItem(urlid) || 0;
      if (id) {
        sessionStorage.setItem("staffid", id);
        let n = parseInt(sessionStorage.getItem(id));
        let total = parseInt(sessionStorage.getItem("total"));
        total = total - n;
        if (total != 0) {
          sessionStorage.setItem("total", total);
        } else {
          sessionStorage.removeItem("total");
        }
        sessionStorage.removeItem(id);
      } else {
        sessionStorage.removeItem("staffid");
      }
      this.$router.push("/" + this.jkl + "/talk/" + urlid);
    },
  },
  mounted() {
    
    this.num1 = Math.floor(Math.random() * 100) + 200;
    this.num2 = Math.floor(Math.random() * 100) + 200;
    this.num3 = Math.floor(Math.random() * 100) + 200;
    window.addEventListener("scroll", this.setnav);
    if (this.host == 0) {
      this.txt = "家园";
    } else {
      this.txt = "易得房";
    }
    sessionStorage.setItem("proid", this.$route.params.id);
    let that = this;
    if (
      sessionStorage.getItem("total") &&
      sessionStorage.getItem("total") !== "NaN"
    ) {
      this.totalnum = parseInt(sessionStorage.getItem("total"));
    }
    $("#foott").css("display", "block");
    window.addEventListener("scroll", this.handleScroll);
    this.ws = this.$store.state.ws;
    this.$store.state.ws.onmessage = function (event) {
      let data = JSON.parse(event.data);
      if (data.action == 301) {
        let urlid = that.$route.params.id;
        if (!sessionStorage.getItem(urlid)) {
          sessionStorage.setItem(urlid, data.fromUserName);
          that.putcard();
        } else {
          if (sessionStorage.getItem(data.fromUserName)) {
            sessionStorage.setItem(
              data.fromUserName,
              parseInt(sessionStorage.getItem(data.fromUserName)) + 1
            );
          } else {
            sessionStorage.setItem(data.fromUserName, 1);
          }
          if (
            sessionStorage.getItem("total") &&
            sessionStorage.getItem("total") != "NaN"
          ) {
            sessionStorage.setItem(
              "total",
              parseInt(sessionStorage.getItem("total")) + 1
            );
            that.totalnum = parseInt(sessionStorage.getItem("total"));
          } else {
            sessionStorage.setItem("total", 1);
            that.totalnum = 1;
          }
        }
      } else if (data.action == 206) {
        that.usernum = data.num.user_num;
        that.looknum = data.num.look_num;
        that.rate = data.num.rate;
        that.stafftel = data.staff.tel;
        that.staffname = data.staff.name;
        that.staffimg = data.staff.img;
        that.talktype = true;
      } else if (data.action == 302) {
        sessionStorage.setItem("currentid", data.sid);
      }
    };
    if (this.kidcode) {
      $cookies.set("kid", this.kidcode);
      $cookies.set("other", this.othercode);
    }
    if (this.phone.split(",").length == 2) {
      this.phonemsg =
        this.phone.split(",")[0].substr(0, 3) +
        " " +
        this.phone.split(",")[0].substr(3, 3) +
        " " +
        this.phone.split(",")[0].substr(6) +
        " <em>转</em> " +
        this.phone.split(",")[1];
    } else {
      this.phonemsg = this.phone;
    }
    $cookies.set("proname", this.abstract.name);
    if (localStorage.getItem(this.$route.params.id)) {
      this.hour = localStorage.getItem(this.$route.params.id);
    } else {
      this.hour = Math.floor(Math.random() * 10) + 1;
      localStorage.setItem(this.$route.params.id, this.hour);
    }

    var date1 = new Date();
    var date2 = new Date(date1);
    date2.setDate(date1.getDate() + 7);
    var time2 = date2.getMonth() + 1 + "月" + date2.getDate() + "日";
    localStorage.setItem(this.$route.params.id + "time", time2);
    this.time = time2;
    this.id = this.$route.params.id;
    let foot = $cookies.get("foot");
    if (foot) {
      foot = foot + "," + that.id;
      foot = foot.split(",");
      let arr = new Set(foot);
      arr = [...arr];
      arr = arr.join(",");
      foot = arr;
    } else {
      foot = that.id;
    }
    $cookies.set("foot", foot);
    this.imgs = this.effects;

    var mySwiper2 = new Swiper(".swiper-topimg", {
      slidesPerView: 1,
      spaceBetween: 0,
      observer: true,
      resistanceRatio: 0.1,
      autoplay: true,
    });
    var mySwiper3 = new Swiper(".swiper-nav", {
      slidesPerView: 5.1,
      spaceBetween: 50,
      resistanceRatio: 0.01,
      slidesOffsetBefore: 14,
      slidesOffsetAfter: 14,
    });
    var mySwiper1 = new Swiper(".swiper-hu", {
      slidesPerView: 2.4,
      spaceBetween: 10,
      observer: true,
      slidesOffsetAfter: 14,
      resistanceRatio: 0.1,
      slidesOffsetBefore: 14,
    });
    var swiper08 = new Swiper(".swiper-map", {
      slidesPerView: 5.5,
      spaceBetween: 10,
      slidesOffsetAfter: 12,
      slidesOffsetBefore: 14,
      slidesOffsetAfter: 12,
    });
    this.$nextTick(() => {
      this.drawline();
      this.mmap();
      // document.getElementById("hh").style.borderBottom = "0";
    });
  },
  beforeDestroy() {
    window.removeEventListener("scroll", this.setnav);
  },
};
</script>
<style lang="less" scoped>
.allbox {
  background-color: #f5f5f5;
  padding-bottom: 1.25rem;
}
.topimg {
  width: 100%;
  height: 13.125rem;
  position: relative;
  padding-top: 2.75rem;
  .swiper-topimg {
    width: 100%;
    height: 100%;
    overflow: hidden;
    .swiper-slide {
      img {
        width: 100%;
        height: 100%;
      }
    }
  }
  .img-top {
    width: 100%;
    height: 100%;
  }
  .imgnum {
    display: block;
    position: absolute;
    width: 3.125rem;
    height: 1.25rem;
    border-radius: 0.625rem;
    background: rgba(0, 0, 0, 0.6);
    font-size: 0.625rem;
    text-align: center;
    line-height: 1.25rem;
    color: #fff;
    right: 0.875rem;
    bottom: 1.125rem;
    z-index: 4;
  }
  .top-heard {
    position: absolute;
    width: 1.375rem;
    top: 3.6875rem;
    right: 1rem;
    z-index: 10;
  }
  .zhe {
    width: 100%;
    height: 100%;
    background-color: #000;
    opacity: 0.4;
    position: absolute;
    top: 0;
    left: 0;
    z-index: 2;
  }
  .top-msg {
    position: absolute;
    width: 100%;
    top: 0.9375rem;
    overflow: hidden;
    border-radius: 1rem;
    width: 18.75rem;
    height: 2rem;
    left: 2.5rem;
    z-index: 3;
    .swiper-slide {
      background: rgba(0, 0, 0, 0.6);
      color: #e1e1e1;
      line-height: 2rem;
      font-size: 0.8125rem;
      img {
        width: 1.125rem;
        margin-right: 0.6875rem;
        margin-bottom: -0.25rem;
        margin-left: 0.9375rem;
      }
      span {
        margin-right: 1.4rem;
      }
      span:nth-of-type(3) {
        margin: 0;
      }
    }
  }
  .imgnum {
    position: absolute;
    width: 3.125rem;
    height: 1.25rem;
    border-radius: 0.625rem;
    background: rgba(0, 0, 0, 0.6);
    text-align: center;
    line-height: 1.25rem;
    color: #fff;
    font-size: 0.625rem;
    right: 4%;
    bottom: 1.5625rem;
    z-index: 5;
  }
  .taps {
    width: 6.4rem;
    height: 1.25rem;
    border-radius: 0.625rem;
    position: absolute;
    bottom: 1.5625rem;
    left: 50%;
    transform: translateX(-50%);
    display: flex;
    z-index: 5;
    line-height: 1.3rem;
    justify-content: space-between;
    p {
      width: 2.875rem;
      color: #646566;
      font-size: 0.625rem;
      text-align: center;
      background-color: #ffffff;
      border-radius: 0.625rem;
    }
    .active {
      background-color: #d1a23d;
      color: #fff;
    }
  }
}
.topnavlist {
  width: 92%;
  padding: 0 4%;
  height: 2.75rem;
  background-color: #fff;
  position: fixed;
  top: 2.75rem;
  left: 0;
  display: flex;
  justify-content: space-around;
  align-items: center;
  z-index: 200;
  p {
    color: #323233;
    font-size: 0.9375rem;
  }
  .active {
    color: #b68826;
    font-weight: bold;
    position: relative;
    span {
      display: block;
      position: absolute;
      left: 50%;
      transform: translateX(-50%);
      width: 1.5625rem;
      height: 0.125rem;
      background-color: #b68826;
      bottom: -0.375rem;
    }
  }
}
.intro {
  width: 84%;
  position: relative;
  left: 4%;
  top: -0.9375rem;
  border-radius: 0.75rem;
  background-color: #fff;
  -webkit-z-index: 5;
  -moz-z-index: 5;
  -ms-z-index: 5;
  -o-z-index: 5;
  z-index: 5;
  padding: 1.375rem 0.9375rem 1.25rem 0.9375rem;
  .intro-top {
    display: flex;
    justify-content: space-between;
    margin-bottom: 1.3125rem;
    .top-left {
      h2 {
        color: #1b1a18;
        font-size: 1.25rem;
        margin-bottom: 0.3125rem;
      }
      p {
        span {
          padding: 0.09375rem 0.4375rem 0.125rem 0.4375rem;
          border-radius: 0.125rem;
          background-color: #f5f5f5;
          color: #7d7e80;
          font-size: 0.6875rem;
          margin-right: 0.125rem;
        }
        .active {
          background-color: #f8efdc;
          color: #b68826;
          margin-right: 0.125rem;
        }
      }
    }
    .top-right {
      display: flex;
      color: #628bb9;
      font-size: 0.75rem;
      padding-top: 0.375rem;
      img {
        width: 1rem;
        height: 1rem;
        margin-right: 0.125rem;
      }
    }
  }
  .intro-fx {
    height: 2.5rem;
    background: url(~assets/content-fx.png);
    background-size: 100%;
    margin-bottom: 0.875rem;
    position: relative;
    p {
      color: #6a4e1a;
      font-size: 0.75rem;
      position: absolute;
      top: 0.75rem;
      right: 0.625rem;
      img {
        width: 0.75rem;
        margin-left: 0.3125rem;
        margin-bottom: -0.125rem;
      }
    }
  }
  .intro-con {
    position: relative;
    .inmore {
      position: absolute;
      color: #969699;
      font-size: 0.8125rem;
      right: 0.375rem;
      top: 1rem;
      img {
        width: 0.375rem;
        margin-left: 0.375rem;
        margin-bottom: -0.0625rem;
      }
    }
    p {
      color: #7d7d80;
      font-size: 0.875rem;
      margin-bottom: 0.5rem;
      span {
        color: #333334;
        font-size: 0.875rem;
      }
      .price {
        color: #ff5454;
        font-size: 1.25rem;
        font-weight: bold;
        i {
          font-weight: 400;
          font-size: 0.625rem;
          font-style: normal;
        }
      }
      .total {
        color: #333334;
        font-size: 1rem;
        i {
          font-style: normal;
          font-size: 0.625rem;
        }
      }
      img {
        width: 2.5rem;
        height: 2.5rem;
        float: right;
        position: absolute;
        top: -0.5rem;
        right: 0;
      }
    }
    p:nth-of-type(4) {
      line-height: 2.125rem;
    }
    p:nth-of-type(3) {
      margin-bottom: 1rem;
      margin-top: 0.0625rem;
    }
    .address {
      display: flex;
      position: relative;
      margin-bottom: 1.125rem;
      span {
        display: block;
        width: 14.5rem;
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
        height: 1.625rem;
      }
    }
  }
  .zhu {
    color: #303233;
    font-size: 0.75rem;
    margin-bottom: 1.125rem;
  }
  .btn {
    display: flex;
    justify-content: space-between;
    button {
      width: 9.5625rem;
      height: 2.25rem;
      border-radius: 0.125rem;
      text-align: center;
      line-height: 2.25rem;
      border: 0;
      background-color: #f8efdc;
      color: #b68826;
      font-size: 0.9375rem;
      font-weight: bold;
      border-radius: 0.375rem;
      img {
        width: 1.125rem;
        margin-bottom: -0.25rem;
        margin-right: 0.25rem;
      }
    }
  }
}
.tel {
  padding: 0 4%;
  margin-bottom: 0.75rem;
  position: relative;
  .telimg {
    border-radius: 0.75rem;
    width: 100%;
  }
  p {
    color: #fcd19e;
    font-size: 1.125rem;
    position: absolute;
    top: 0.6875rem;
    font-weight: bold;
    left: 1.6rem;
    /deep/em {
      font-style: normal;
      font-size: 1rem;
    }
  }
  button {
    width: 5.375rem;
    height: 1.9375rem;
    border-radius: 0.9375rem;
    background-color: #fcd19e;
    display: flex;
    justify-content: center;
    align-items: center;
    border: 0;
    outline: none;
    color: #4c4a41;
    font-size: 0.8125rem;
    position: absolute;
    right: 1.5625rem;
    top: 1rem;
    img {
      width: 1rem;
      margin-right: 0.0625rem;
    }
  }
}
.line {
  width: 100%;
  height: 0.625rem;
  background-color: #f7f7f7;
}
.te {
  position: relative;
  padding: 0 0.9375rem;
  margin: 0 0.9375rem;
  border-radius: 0.75rem;
  background-color: #fff;
  margin-top: 0.625rem;
  .te-tit {
    width: 5rem;
    right: 0.6875rem;
    top: 1.0625rem;
  }
  h3 {
    color: #19191a;
    font-size: 1.0625rem;
    margin-bottom: 0.875rem;
    padding-top: 1rem;
    font-weight: bold;
    padding-bottom: 0.8125rem;
    border-bottom: 0.03125rem solid #ededed;
    span {
      color: #969699;
      font-size: 0.625rem;
      font-weight: 400;
      font-weight: 400;
    }
    img {
      width: 1.125rem;
      margin-bottom: -0.125rem;
    }
  }
  ul {
    height: auto;
    overflow: hidden;
    li {
      display: flex;
      align-items: center;
      margin-bottom: 0.8125rem;
      .left {
        .msg {
          color: #333333;
          font-size: 0.9375rem;
          margin-bottom: 0.1875rem;
        }
        .pri {
          display: flex;
          .tepri {
            color: #ff6040;
            font-size: 0.75rem;
            margin-right: 0.25rem;
            span {
              font-size: 1.25rem;
              font-weight: bold;
            }
          }
          .oldpri {
            color: #949494;
            font-size: 0.75rem;
            margin-top: 0.375rem;
            span {
              text-decoration: line-through;
            }
          }
        }
      }
      .right {
        width: 4.6875rem;
        height: 1.625rem;
        background: linear-gradient(270deg, #ff4b2d, #ffb753);
        border-radius: 0.8125rem;
        text-align: center;
        line-height: 1.625rem;
        color: #fff;
        font-size: 0.75rem;
        margin-left: auto;
        margin-left: auto;
      }
    }
  }
  .zhe {
    position: absolute;
    width: 92%;
    height: 3.125rem;
    z-index: 1;
    background: linear-gradient(
      0deg,
      rgba(255, 255, 255, 1),
      rgba(255, 255, 255, 0)
    );
    bottom: 6.25rem;
    left: 4%;
    text-align: center;
    line-height: 3.125rem;
    img {
      width: 1.5rem;
    }
  }
  .bom {
    width: 100%;
    /deep/.van-notice-bar {
      padding: 0;
      z-index: 0;
    }
    button {
      width: 100%;
      height: 2.5rem;
      border-right: 0.25rem;
      background: #f8efdc;
      border: 0;
      text-align: center;
      line-height: 2.5rem;
      font-size: 0.9375rem;
      color: #b68826;
      font-weight: bold;
      border-right: 0.25rem;
      margin-top: 0.375rem;
      margin-bottom: 1.25rem;
      border-radius: 0.375rem;
    }
  }
}
.hui {
  padding: 1.125rem 4% 1.25rem 4%;
  background-color: #fff;
  margin: 0 0.9375rem;
  border-radius: 0.75rem;
  h3 {
    color: #1f1f1f;
    font-size: 1.0625rem;
    line-height: 1.375rem;
    img {
      width: 0.9375rem;
      margin-bottom: -0.125rem;
    }
  }
  .hui-con {
    width: 100%;
    height: 4.375rem;
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
  .phone-huo {
    position: relative;
    margin-top: .875rem;
    img {
      width: 100%;
    }
    p {
      position: absolute;
      font-size: .75rem;
      color: #fff;
      right: 1.25rem;
      bottom: .625rem;
    }
    button {
      width: 4rem;
      height: 1.5rem;
      border-radius: .75rem;
      background-color: #fff;
      text-align: center;
      line-height: 1.5rem;
      border: 0;
      outline: none;
      position: absolute;
      right: .9375rem;
      top: 1.4375rem;
      color: #153870;
      font-size: .75rem;
    }
  }
}
.hus {
  padding-bottom: 1.25rem;
  margin: 0 0.9375rem;
  border-radius: 0.75rem;
  background-color: #fff;
  margin-top: 0.625rem;
  h5 {
    color: #19191a;
    font-size: 1.0625rem;
    padding: 0 4%;
    margin-bottom: 1.0625rem;
    padding-top: 1.0625rem;
    span {
      float: right;
      color: #646466;
      font-size: 0.875rem;
      font-weight: 400;
      img {
        width: 0.875rem;
        margin-left: 0.25rem;
        margin-bottom: -0.0625rem;
      }
    }
  }
  .hu-msg {
    overflow: hidden;
  }
  .swiper-hu {
    overflow: hidden;
  }
  .swiper-slide {
    border-radius: 0.125rem;
    .hu-top {
      background-color: #f7f7f7;
      overflow: hidden;
      border-radius: 0.375rem;
      height: 6.25rem;
      display: flex;
      justify-content: center;
      align-items: center;
      position: relative;
      p {
        width: 4.6875rem;
        height: 1.0625rem;
        position: absolute;
        bottom: 0;
        right: 0;
        color: #63696e;
        font-size: 0.5625rem;
        text-align: right;
        line-height: 1.0625rem;
        background: linear-gradient(
          -90deg,
          rgba(0, 0, 0, 0.12),
          rgba(205, 205, 205, 0)
        );
        img {
          height: 0.75rem;
          width: 0.75rem;
          margin-bottom: -0.125rem;
        }
      }
      img {
        height: 6.6875rem;
        width: 5.1875rem;
      }
    }
    .hu-bom {
      padding-bottom: 0.75rem;
      padding-top: 0.625rem;
      border-radius: 0 0 0.125rem 0.125rem;
      padding-right: 0.75rem;
    }
    .name {
      color: #323233;
      font-size: 0.875rem;
      font-weight: bold;
      margin-bottom: 0.1875rem;
      display: flex;
      align-items: center;
      span {
        display: inline-block;
        color: #ffffff;
        font-size: 0.625rem;
        width: 1.6875rem;
        height: 0.875rem;
        text-align: center;
        line-height: 0.875rem;
        background-color: #d1a23d;
        border-radius: 0.1875rem;
        margin-left: 0.625rem;
        font-weight: normal;
      }
    }
    .type {
      color: #7d7e80;
      font-size: 0.75rem;
      margin-bottom: 0.125rem;
      .t1 {
        margin-right: 0.75rem;
      }
    }
    .huprice {
      color: #ff6040;
      font-size: 0.625rem;
      margin-bottom: 0.3125rem;
      span {
        font-size: 1rem;
        font-weight: bold;
      }
    }
  }
  button {
    width: 19.6875rem;
    height: 2.5rem;
    border-radius: 0.375rem;
    background-color: #f8efdc;
    color: #b68826;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 0.9375rem;
    font-weight: bold;
    border: 0;
    margin-left: 0.9375rem;
  }
}
.imgbox {
  position: fixed;
  width: 100%;
  top: 0;
  background: rgba(0, 0, 0, 0.6);
  height: 100vh;
  z-index: 10000;
  img {
    width: 100%;
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
  }
}
.dynamic {
  padding: 0 0.9375rem;
  margin: 0 0.9375rem;
  background-color: #fff;
  border-radius: 0.75rem;
  margin-top: 0.625rem;
  padding-bottom: 1.25rem;
  h3 {
    color: #121212;
    font-size: 1.0625rem;
    padding: 1.0625rem 0 0.9375rem 0;
    position: relative;
    .tt {
      position: absolute;
      width: 2rem;
      top: 0.9375rem;
      left: 4.25rem;
    }
    span {
      color: #646466;
      font-size: 0.875rem;
      float: right;
      font-weight: 400;
      img {
        width: 0.875rem;
      }
    }
  }
  ul {
    li {
      margin-bottom: 1.125rem;
      p {
        color: #646566;
        font-size: 0.875rem;
        line-height: 1.3125rem;
        height: 2.375rem;
        display: -webkit-box;
        -webkit-box-orient: vertical;
        -webkit-line-clamp: 2;
        overflow: hidden;
      }
      span {
        color: #969799;
        font-size: 0.75rem;
        margin-top: 0.375rem;
        display: block;
      }
      .dynamicimgs {
        margin-top: 0.4375rem;
        img {
          width: 6.6875rem;
          height: 4.375rem;
          border-radius: 0.375rem;
        }
      }
    }
    li:last-child {
      margin-bottom: 0.75rem;
    }
  }
  .btn {
    padding-top: 0.9375rem;
    border-top: 0.03125rem solid #ededed;
    display: flex;
    input {
      width: 12.8125rem;
      height: 2.75rem;
      border: 0;
      outline: none;
      background: #f5f5f5;
      border-radius: 0.375rem;
      padding-left: 0.9375rem;
    }
    button {
      width: 5rem;
      height: 2.75rem;
      background-color: #d1a23d;
      text-align: center;
      line-height: 2.75rem;
      font-weight: bold;
      color: #fefffe;
      font-size: 0.9375rem;
      border: 0;
      border-radius: 0.375rem;
      margin-left: auto;
    }
  }
}
.zixun {
  padding: 1.0625rem 0.9375rem 1.125rem 0.9375rem;
  margin: 0 0.9375rem;
  background-color: #fff;
  border-radius: 0.75rem;
  margin-top: 0.625rem;
  h3 {
    color: #17181a;
    margin-bottom: 0.6875rem;
    font-size: 1.0625rem;
    span {
      float: right;
      color: #628bb9;
      font-size: 0.8125rem;
      font-weight: normal;
      img {
        width: 0.875rem;
        margin-bottom: -0.0625rem;
      }
    }
  }
  .xun-icon {
    margin-bottom: 1.6875rem;
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
    margin-bottom: 1.375rem;
    .peoimg {
      width: 2.5rem;
      height: 2.5rem;
      border-radius: 50%;
    }
    .peo-msg {
      position: relative;
      margin-left: 0.75rem;
      top: -0.25rem;
      margin-right: 1.125rem;
      h6 {
        color: #323233;
        font-size: 1rem;
        font-weight: 400;
        margin-bottom: 0.3125rem;
        span {
          display: inline-block;
          padding: 0.03125rem 0.25rem 0.0625rem 0.25rem;
          color: #fff;
          background-color: #ff7519;
          font-size: 0.625rem;
          margin-left: 0.25rem;
          font-weight: 400;
          border-radius: 0.1875rem;
        }
      }
      p {
        color: #7d7d80;
        font-size: 0.75rem;
      }
    }
    .psm {
      margin-right: 1.5625rem;
    }
    a {
      justify-content: flex-end;
    }
    .peoicon {
      width: 2.5rem;
      height: 2.5rem;
      margin-left: 1.25rem;
    }
    .peoicon:nth-of-type(2) {
      margin-left: 0;
    }
  }
  .peo:nth-of-type(2) {
    margin-bottom: 0;
  }
}
.ziliao {
  padding: 1.125rem 4% 1.25rem 4%;
  margin: 0 0.9375rem;
  margin-top: 0.625rem;
  background-color: #fff;
  border-radius: 0.75rem;
  position: relative;
  h3 {
    color: #1f1f1f;
    font-size: 1.0625rem;
    margin-bottom: 1.125rem;
  }
  .tab {
    margin-bottom: 1.25rem;
    display: flex;
    p {
      position: relative;
      width: 4.875rem;
      height: 1.75rem;
      text-align: center;
      line-height: 1.75rem;
      margin-right: 1rem;
      border-radius: 0.875rem;
      font-size: 0.875rem;
      color: #646466;
      background-color: #f5f5f5;
    }
    .active {
      background: linear-gradient(90deg, #d1a23d, #e9d28b);
      color: #fff;
    }
  }
  .liao-msg {
    // height: 7.5rem;
    border-radius: 0.375rem;
    background-color: #f7f7f7;
    padding: 0 0.875rem;
    margin-bottom: 1.25rem;
    padding-bottom: 0.8rem;
    p {
      color: #4b4b4d;
      font-size: 0.875rem;
      line-height: 1.25rem;
      padding-top: 0.9375rem;
      height: 2.4375rem;
      display: -webkit-box;
      -webkit-box-orient: vertical;
      -webkit-line-clamp: 2;
      overflow: hidden;
    }
  }
  .zhe {
    position: absolute;
    bottom: 4.6875rem;
    left: 0;
    width: 100%;
    height: 4.0625rem;
    background: linear-gradient(0deg, #ffffff 50%, rgba(255, 255, 255, 0));
    display: flex;
    align-items: center;
    justify-content: center;
    img {
      width: 1rem;
    }
  }
  button {
    width: 100%;
    height: 2.5rem;
    border-radius: 0.375rem;
    background-color: #f8efdc;
    text-align: center;
    line-height: 2.5rem;
    color: #b68826;
    font-size: 0.9375rem;
    font-weight: bold;
    border: 0;
    outline: none;
  }
}
.newprice {
  padding: 1.25rem 4%;
  position: relative;
  margin: 0 4%;
  border-radius: 0.75rem;
  background-color: #fff;
  margin-top: 0.625rem;
  h3 {
    color: #19191a;
    font-size: 1.0625rem;
    display: flex;
    p {
      color: #969699;
      font-size: 0.625rem;
      font-weight: 400;
      margin-left: 0.3125rem;
    }
  }
  p {
    text-align: center;
    color: #5c5c5c;
    font-size: 0.625rem;
    margin-bottom: 0.875rem;
    margin-top: 0.25rem;
  }
  .tab {
    position: relative;
    height: 9.75rem;
    overflow: hidden;
    table {
      color: #5c5c5c;
      font-size: 0.75rem;
      width: 100%;
      border-top: 0.5px solid #e6e6ec;
      border-left: 0.5px solid #e6e6ec;
      border-right: 0.5px solid #e6e6ec;
      border-radius: 0.375rem;
      .thead {
        background-color: #f2f2f2;
        border-right: 0.5px solid #e6e6ec;
        border-bottom: 0.5px solid #e6e6ec;
        th {
          border: 0;
        }
      }
      tr {
        height: 1.9375rem;
        line-height: 1.9375rem;
        th {
          width: 33%;
          text-align: center;
          border-right: 0.5px solid #e6e6ec;
          border-bottom: 0.5px solid #e6e6ec;
        }
        th:nth-of-type(3) {
          border-right: 0;
        }
        td {
          width: 33%;
          text-align: center;
          border-right: 0.5px solid #e6e6ec;
          border-bottom: 0.5px solid #e6e6ec;
        }
        td:nth-of-type(3) {
          border-right: 0;
        }
      }
    }
    .zhe {
      position: absolute;
      width: 100%;
      height: 3.125rem;
      background: linear-gradient(
        0deg,
        rgba(255, 255, 255, 1),
        rgba(255, 255, 255, 0)
      );
      bottom: 0;
      text-align: center;
      line-height: 3.125rem;
      img {
        width: 1.5rem;
        position: relative;
        top: 0.5rem;
      }
    }
  }
  /deep/.van-notice-bar {
    padding: 0;
    z-index: 0;
  }
  .notice-swipe {
    height: 2.5rem;
    line-height: 2.5rem;
  }
  button {
    width: 100%;
    height: 2.5rem;
    background-color: #f8efdc;
    text-align: center;
    line-height: 2.5rem;
    color: #b68826;
    font-size: 0.875rem;
    font-weight: bold;
    border: 0;
    margin-top: 0.8rem;
    border-radius: 0.375rem;
    img {
      width: 1.125rem;
      margin-bottom: -0.1875rem;
      margin-right: 0.125rem;
    }
  }
}
.zhou {
  // padding: 0 4%;
  margin: 0 0.9375rem;
  background-color: #fff;
  margin-top: 0.625rem;
  border-radius: 0.75rem;
  h4 {
    padding: 0 4%;
    padding-top: 1.125rem;
    font-size: 1.0625rem;
    color: #1f1f1f;
    line-height: 1.375rem;
    margin-bottom: 0.6875rem;
  }
  .txt {
    color: #969699;
    font-size: 0.9375rem;
    margin-bottom: 0.75rem;
    padding: 0 4%;
    span {
      color: #323233;
      margin-left: 0.125rem;
    }
    .talk {
      color: #b68826;
    }
    img {
      width: 1.125rem;
      margin-bottom: -0.125rem;
      margin-left: 0.25rem;
    }
  }
  #map {
    width: 92%;
    margin-left: 4%;
    height: 9.375rem;
    border-radius: 0.375rem;
    margin-bottom: 0.8125rem;
  }
  .swiper-map {
    overflow: hidden;
    padding-bottom: 1rem;
    padding-top: 0.375rem;
    margin-bottom: 0.3125rem;
    .tegood {
      color: #4d4d4d;
      font-size: 1rem;
    }
    .active {
      color: #b68826;
      position: relative;
      i {
        width: 1.5625rem;
        height: 0.125rem;
        border-radius: 0.0625rem;
        background-color: #b68826;
        position: absolute;
        display: block;
        bottom: -0.375rem;
        left: 0;
      }
    }
  }
  .map-msg {
    padding-top: 1.125rem;
    border-top: 0.5px solid #f5f5f5;
    padding: 0 4%;
    .map-msg-con {
      padding-top: 1.0625rem;
      /deep/li {
        list-style: none;
        margin-bottom: 1.125rem;
        h5 {
          color: #1a1a1a;
          font-size: 0.25rem;
          margin-bottom: 0.1875rem;
          font-weight: 400;
        }
        .mapss {
          color: #999999;
          font-size: 0.8125rem;
          display: flex;
          p {
            flex: 1;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
          }
          span {
            margin-left: auto;
            img {
              width: 0.75rem;
              margin-right: 0.125rem;
            }
          }
        }
      }
    }
    .map-msg-con /deep/ li {
      list-style: none;
      margin-bottom: 0.75rem;
      h5 {
        color: #1a1a1a;
        font-size: 0.9375rem;
        margin-bottom: 0.1875rem;
      }
      p {
        color: #999999;
        font-size: 0.8125rem;
        span {
          float: right;
          img {
            width: 0.75rem;
            margin-right: 0.125rem;
          }
        }
      }
    }
    .map-tishi {
      height: 10.6875rem;
      text-align: center;
      line-height: 10.6875rem;
      font-size: 1rem;
    }
  }
  button {
    margin-top: 1rem;
    width: 92%;
    margin-left: 4%;
    height: 2.5rem;
    border-radius: 0.375rem;
    background: #f8efdc;
    color: #b68826;
    font-size: 0.9375rem;
    text-align: center;
    line-height: 2.5rem;
    font-weight: bold;
    border: 0;
    margin-bottom: 1.25rem;
  }
}
.other {
  padding: 1.25rem 4% 0 4%;
  margin: 0 4%;
  border-radius: 0.75rem;
  background-color: #fff;
  margin-top: 0.625rem;
  padding-bottom: 1.25rem;
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
      border-radius: 0.375rem;
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
          color: #b68826;
          font-size: 0.6875rem;
          float: right;
          padding: 0.125rem 0.375rem 0.1875rem 0.375rem;
          background-color: #f8efdc;
          border-radius: 0.1875rem;
          font-weight: 400;
        }
      }
      .pro-price {
        color: #7a7a7a;
        font-size: 0.75rem;
        margin-bottom: 0.1875rem;
        span {
          color: #ff6040;
          font-size: 1rem;
          font-weight: bold;
        }
        i {
          font-style: normal;
          color: #ff6040;
        }
      }
      .attr {
        color: #646466;
        font-size: 0.75rem;
        margin-bottom: 0.1875rem;
      }
      .pro-icon {
        display: flex;
        .pro-icon-zhuang {
          color: #3eacf0;
          font-size: 0.6875rem;
          padding: 0.1875rem 0.375rem;
          background-color: #ebf8ff;
          margin-right: 0.375rem;
          border-radius: 0.1875rem;
        }
        .pro-icon-type {
          color: #888a8f;
          font-size: 0.6875rem;
          padding: 0.1875rem 0.375rem;
          border-radius: 0.1875rem;
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
.luck {
  position: fixed;
  right: 0.9375rem;
  bottom: 7.5rem;
  z-index: 1001;
  transition: right 0.5s linear;
  img {
    width: 3.125rem;
  }
}
.lucked {
  right: -1.75rem;
}
.dian {
  padding: 0 4%;
  margin: 0 0.9375rem;
  background-color: #fff;
  border-radius: 0.75rem;
  margin-top: 0.625rem;
  h3 {
    color: #121212;
    padding: 1rem 0 1.25rem 0;
    font-size: 1.0625rem;
    span {
      float: right;
      color: #969699;
      font-size: 0.875rem;
      font-weight: 400;
      img {
        width: 0.875rem;
        margin-bottom: -0.125rem;
      }
    }
  }
  ul {
    li {
      margin-bottom: 1.375rem;
      .dian-top {
        overflow: hidden;
        .peo {
          width: 2.25rem;
          height: 2.25rem;
          border-radius: 50%;
          margin-right: 0.625rem;
          float: left;
        }
        .top-con {
          float: left;
          h6 {
            color: #323333;
            font-size: 0.875rem;
            span {
              color: #92949a;
              font-size: 0.8125rem;
              margin-left: 0.1875rem;
              font-weight: 400;
            }
          }
          p {
            color: #92949a;
            font-size: 0.8125rem;
            margin-top: 0.3rem;
            line-height: 1.3125rem;
          }
        }
        .top-right {
          float: right;
          color: #96989e;
          font-size: 0.75rem;
          img {
            width: 1rem;
            margin-left: 0.1875rem;
            margin-bottom: -0.125rem;
          }
        }
        .active {
          color: #b68826;
        }
      }
      .dian-bom {
        color: #333333;
        font-size: 0.875rem;
        line-height: 1.25rem;
        margin-top: 0.6875rem;
      }
    }
  }
  .tishi {
    font-size: 0.875rem;
    text-align: center;
    margin-top: 0.625rem;
    margin-bottom: 0.625rem;
  }
  button {
    width: 100%;
    height: 2.5rem;
    border-radius: 0.375rem;
    background-color: #f8efdc;
    text-align: center;
    line-height: 2.5rem;
    color: #b68826;
    font-size: 0.9375rem;
    font-weight: bold;
    border: 0;
    margin-bottom: 1.25rem;
    outline: none;
  }
}
.wen {
  padding: 0 4%;
  margin: 0 0.9375rem;
  margin-top: 0.625rem;
  border-radius: 0.75rem;
  background-color: #fff;
  h3 {
    color: #121212;
    font-size: 1.0625rem;
    padding: 1.125rem 0 1.5rem 0;
    span {
      font-size: 0.875rem;
      font-weight: 400;
      color: #969699;
      float: right;
      img {
        width: 0.875rem;
        margin-left: 0.25rem;
        margin-bottom: -0.125rem;
      }
    }
  }
  ul {
    li {
      margin-bottom: 1.25rem;
      .con {
        color: #323333;
        font-size: 0.875rem;
        line-height: 1.375rem;
        span {
          padding: 0.125rem 0.1875rem 0.1875rem 0.1875rem;
          border-radius: 0.125rem;
          background-color: #ff5454;
          color: #fff;
          font-size: 0.625rem;
          margin-right: 0.4rem;
        }
      }
      .num {
        color: #9a9a9a;
        font-size: 0.8125rem;
      }
    }
  }
  .tishi {
    font-size: 0.875rem;
    text-align: center;
    margin-top: 0.625rem;
    margin-bottom: 0.625rem;
  }
  button {
    width: 100%;
    height: 2.5rem;
    border-radius: 0.375rem;
    text-align: center;
    line-height: 2.5rem;
    font-weight: bold;
    background-color: #f8efdc;
    color: #b68826;
    font-size: 0.9375rem;
    border: 0;
    margin-bottom: 1.25rem;
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
.talkbox {
  position: fixed;
  bottom: 0;
  z-index: 3000;
  height: 13.75rem;
  width: 100%;
  background-color: #fff;
  box-shadow: 0px 0px 0.5625rem 0px rgba(52, 71, 87, 0.24);
  .del {
    width: 1rem;
    height: 1rem;
    position: absolute;
    top: 0.9375rem;
    right: 0.9375rem;
  }
  .peo {
    padding-top: 1.5625rem;
    padding-left: 1.9375rem;
    display: flex;
    margin-bottom: 1.125rem;
    .left {
      margin-right: 0.625rem;
      overflow: hidden;
      height: 3rem;
      border-radius: 50%;
      img {
        width: 3rem;
      }
    }
    .right {
      h3 {
        color: #1f1f1f;
        font-size: 1.0625rem;
        margin-bottom: 0.375rem;
        span {
          font-weight: 400;
          color: #7495bd;
          font-size: 0.6875rem;
          padding: 0.125rem 0.3125rem 0.15625rem 0.3125rem;
          background-color: #f2f8ff;
          border-radius: 0.09375rem;
        }
      }
      p {
        color: #646466;
        font-size: 0.8125rem;
      }
    }
  }
  .msg {
    display: flex;
    margin-bottom: 1.75rem;
    .li {
      width: 33%;
      text-align: center;
      border-right: 0.03125rem solid #f0f0f2;
      .num {
        color: #121212;
        font-size: 0.625rem;
        margin-bottom: 0.25rem;
        span {
          color: #121212;
          font-size: 1.125rem;
          font-weight: bold;
        }
      }
      .txt {
        color: #646466;
        font-size: 0.6875rem;
      }
    }
  }
  .btn {
    button {
      width: 9.375rem;
      height: 2.5rem;
      border-radius: 0.25rem;
      text-align: center;
      line-height: 2.5rem;
      font-size: 0.9375rem;
      border: 0;
      outline: none;
    }
    button:nth-of-type(1) {
      background: linear-gradient(270deg, #d1a23d, #ebbf5f);
      margin-left: 1.9375rem;
      color: #fff;
    }
    button:nth-of-type(2) {
      border: 0.03125rem solid #b68826;
      background-color: #f8efdc;
      color: #b68826;
      margin-left: 0.625rem;
    }
  }
}
.my-infos {
  margin: 0 4%;
  border-radius: 0.75rem;
  background-color: #fff;
  margin-top: 0.625rem;
  h4 {
    color: #121212;
    font-size: 1rem;
    font-weight: bold;
    padding: 0 4%;
    padding-top: 1.125rem;
    margin-bottom: 1.25rem;
    span {
      color: #646466;
      font-size: 0.875rem;
      float: right;
      font-weight: normal;
      img {
        width: 0.875rem;
        margin-left: 0.25rem;
        margin-bottom: -0.125rem;
      }
    }
  }
  ul {
    padding: 0 4%;
    li {
      color: #323233;
      font-size: 0.875rem;
      line-height: 1.3rem;
      padding-bottom: 0.8125rem;
      border-bottom: 0.0625rem solid #f2f2f2;
      margin-bottom: 0.875rem;
    }
    li:before {
      content: "";
      display: inline-block;
      width: 0.1875rem;
      height: 0.1875rem;
      background-color: #333;
      border-radius: 50%;
      margin-right: 0.3125rem;
      vertical-align: middle;
    }
    a:last-child li {
      border: 0;
      margin-bottom: 0;
    }
  }
}
</style>
