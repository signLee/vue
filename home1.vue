<!--首页-->
<template>
  <div class="home">
    <h3 class="line">vue--那些我踩过的不起眼的小坑</h3>
    <p class="line MB20">首推官方推荐第三方插件库 awesome-vue</p>

    <!-- 通过v-html新增插入的dom上的类名只有全局的类名会生效，局部的没有生效，如果想要局部的生效写样式的时候需要做一些调整用到 >>>的方式 -->
    <P class="line" v-html="testHtml"></P>

  <!-- .native主要用于给自定义的组件添加原生事件 -->
     <upLoadImg ref="upLoad" @click="test" :imgPath="imgPath" />
     <!-- <upLoadImg @click.native="test" :imgPath="imgPath" /> -->

 <!-- 阻止冒泡 stop -->
 <!-- @click.self  -限制将事件绑定到自己身上 子事件冒泡也不会触发 -->
    <div @click="test1">
      父级
        <div @click.self="test2">
            子级
             <div @click="test2">
              子级111
            </div>
        </div>
    </div>
   
     <!-- prevent使用 -->
    <router-link to="/login">
      <div @click="test3">prevent</div>
    </router-link>
     <!-- <router-link to="/login">
      <div @click.prevent="test3">prevent</div>
    </router-link> -->
   
   <!-- 关于图片引用 -->
    <img src="./logo.png" />
    <img :src="logo" />

    <!-- 关于元素抓取 -->
    <div ref="testRef" @click="getElements">抓取元素测试</div>

    <!-- 关于router-link  当前active状态的会带有active的类名-->
    <p>关于router-link</p>

    <p v-if="true">关于v-if和v-show的对比</p>
    <p v-show="false">关于v-if和v-show的对比</p>

    <!-- 关于class绑定 -->
    <p class="line" :class="{active:falseFlag==false}">关于class绑定</p>
    <p class="line" :class="[myClass]">关于class绑定</p>
    <p class="line" :class="getClass()">关于class绑定{{getClass()}}</p>
    <!-- 应用场景 排名 -->
    <ul>
      <li v-for="(item,index) in testArr" :class="['aaa'+item,{'active':index==2}]">{{item}}</li>
    </ul>

    <!-- 关于params&query -->
    <p @click="paramsType">params</p>
    <p @click="queryType">query</p>

    <!-- 关于路由跳转，各种路径 -->
    <div>
      <router-link to="/home">to="/home-------->表示跳转到根目录下的home组件"</router-link>
    </div>
    <div>
      <router-link to="home">to="home"--------->表示跳转到当前目录下的home组件</router-link>
    </div>
    <div>
       <router-link :to="home">:to="home"------->表示跳转到home这个变量组件</router-link>
    </div>
    <div>
      <router-link :to="'home'">:to="'home'"--->表示跳转到当前目录下的home组件</router-link>
    </div>
    <div>
      <router-link :to="{path:'/home',param:{paramsData:'123'}}">:to="{path:'/home',param:{paramsData:'123'}"--->表示跳转到根目录下的home组件并传参数</router-link>
    </div>

    <!-- 关于$nextTick -->
    <p ref="nextTickTest" @click="test4">{{msg}}</p>

    <!-- 关于$set -->
    <p v-for="item in items">
        {{item.message}}
    </p>
    <p @click="addData">点击添加$set数据</p>

  </div>
</template>

<script type="text/ecmascript-6">
import upLoadImg from "base/upLoadImg/ulLoadImg";
export default {
  data() {
    return {
      testHtml: '<p class="line MB20 red">这里是红色的字</p>',
      imgPath: [], //产品角标（1.1）
      logo: require("./logo.png"),
      falseFlag: false,
      testArr: [1, 2, 3, 4],
      myClass: "red",
      home: "home",
      msg: "这个是nextTick的内容",
      items: [
        { message: "Test one", id: "1" },
        { message: "Test two", id: "2" },
        { message: "Test three", id: "3" }
      ]
    };
  },
  methods: {
    test() {
      console.log(111);
    },
    test1() {
      console.log("test1");
    },
    test2() {
      console.log("test2");
    },
    test3() {
      console.log("test3");
    },
    getElements() {
      console.log(this.$refs.testRef);
      // console.log(this.$refs.upLoad)
      // console.log(this.$refs.upLoad.$el)//抓取组件元素
      console.log(this.$refs.upLoad.beforeAvatarUpload); //抓取组件元素
    },
    getClass() {
      return "aaa";
    },
    paramsType() {
      this.$router.push({ path: "/login", params: { test: "22222222" } });
      // this.$router.push({name:'login',params:{test:'22222222'}})
      // params类似于post
      // params只能用name来引入路由的时候可以获取到params里的参数
    },
    queryType() {
      // this.$router.push({path:'/login',query:{test:'22222222'}})
      this.$router.push({ name: "login", query: { test: "22222222" } });
      // 类似于get
    },
    test4() {
      this.msg = "11111";
      console.log(this.$refs.nextTickTest.innerText);
      this.$nextTick(() => {
        console.log(this.$refs.nextTickTest.innerText);
      });
    },
    addData(){
      this.items[0]={ message: "这个是修改后的数据", id: "1" };
      // this.$set(this.items,0,{ message: "这个是修改后的数据", id: "1" })
    }
  },
  mounted() {},
  components: { upLoadImg }
};
</script>

<style scoped>
.home {
  width: auto;
  height: calc(100vh - 50px);
  overflow: scroll;
  margin: 20px;
  padding-bottom: 50px;
  padding-left: 20px;
  box-sizing: border-box;
  background: #fff;
}
.line {
  height: 30px;
  line-height: 30px;
  box-sizing: border-box;
}
.red {
  color: red;
}
/* >>> .red{
  color: blue;
} */
</style>
