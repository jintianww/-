<template>
  <div class="about">
    <myheader :title="'许愿墙'"></myheader>
    <p>
      <input v-model="user" type="text" placeholder="输入你的愿望" />
    </p>
    <div class="imgs">
      <img @click="myurl=item.img" :class="myurl==item.img?'red':''" v-for="(item,i) in list" :key="i" :src="item.img" alt />
    </div>
    <div class="cny">
      <button @click="kai" :class="{sky:disabled}">{{yuan}}</button>
    </div>
    <div class="mixi">
      <div v-for="item,i in mylist" @click="upda(item)" :key="i" :style="{backgroundImage:'url('+item.img+')', left:item.left+'px', top:item.top+'px'}" @touchstart.stop="touchstart(item.id)" @touchmove="touchmove">
        <span @click.stop="del(i)">X</span>
        <p>{{item.user}}</p>
      </div>
    </div>
  </div>
</template>

<script>
let ctimer;
import myheader from "../components/myheader.vue";
import axios from "axios";
import { ref, reactive, toRefs, watchEffect, watch } from "vue";
export default {
  setup() {
    let arr = reactive({
      list: [],
    });
    let user = ref("");
    let myurl = ref("");
    let mylist = reactive(JSON.parse(localStorage.mylist || "[]"));
    let disabled = ref(false);
    let yuan = ref("在想想");
    let myid = ref("");
    axios.get("/list.json").then((res) => {
      // console.log(res.data);
      arr.list = res.data;
    });
    watchEffect(() => {
      if (user.value != "" && myurl.value != "") {
        disabled.value = true;
        yuan.value = "许愿";
        if (myid.value != "") {
          yuan.value = "修改";
        }
      } else {
        disabled.value = false;
        yuan.value = "在想想";
      }
    });
    function kai() {
      if (yuan.value == "修改") {
        mylist.forEach((item) => {
          if (item.id == myid.value) {
            item.user = user.value;
            item.img = myurl.value;
          }
        });
      }
      myid.value = "";
      if (yuan.value == "许愿") {
        if (mylist.length > 5) {
          alert("最多只能许6个愿望");
          return;
        }
        let x = Math.ceil(Math.random() * window.innerWidth);
        let y = Math.ceil(Math.random() * window.innerHeight);
        if (x <= 0) x = 0;
        if (y <= 0) y = 0;
        if (x >= window.innerWidth - 120) x = window.innerWidth - 120;
        if (y > window.innerHeight - 120) y = window.innerHeight - 120;
        mylist.push({
          id: new Date().getTime(),
          img: myurl.value,
          user: user.value,
          left: x,
          top: y,
        });
      }
      myurl.value = "";
      user.value = "";
    }
    function del(i) {
      mylist.splice(i, 1);
    }
    function upda(item) {
      myurl.value = item.img;
      user.value = item.user;
      myid.value = item.id;
      yuan.value = "修改";
    }
    let mytd = ref(0);
    function touchstart(e) {
      mytd.value = e;
    }
    function touchmove(e) {
      let max = e.target.offsetWidth;
      let maY = e.target.offsetHeight;
      let xx = e.targetTouches[0].pageX - max / 2;
      let yy = e.targetTouches[0].pageY - maY / 2;
      if (xx <= 0) xx = 0;
      if (yy <= 0) yy = 0;
      if (xx > window.innerWidth - max) xx = window.innerWidth - max;
      if (yy > window.innerHeight - maY) yy = window.innerHeight - maY;

      mylist.forEach((item) => {
        if (item.id == mytd.value) {
          item.left = xx;
          item.top = yy;
        }
      });
    }
    watch(
      () => mylist,
      () => {
        localStorage.mylist = JSON.stringify(mylist);
      },
      { deep: true }
    );
    return {
      ...toRefs(arr),
      yuan,
      user,
      myurl,
      mylist,
      disabled,
      kai,
      del,
      upda,
      touchstart,
      touchmove,
    };
  },
  components: { myheader },
};
</script>

<style lang="scss" scoped>
input[type="text"] {
  height: 36px;
  margin: 10px;
  text-indent: 12px;
  width: 260px;
}
.imgs {
  display: flex;
  justify-content: space-around;
  img {
    width: 60px;
    height: 60px;
  }
}
img {
  width: 120px;
  height: 120px;
}
.cny {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 50px;
  button{
    padding: 6px 12px;
  }
}
.red {
  border: 2px solid red;
}
.sky {
  background-color: springgreen;
  color: white;
}
.mixi {
  display: flex;
  flex-wrap: wrap;
  div {
    background-repeat: no-repeat;
    background-size: cover;
    width: 120px;
    height: 120px;
    position: absolute;
    span {
      float: right;
      margin-right: 6px;
      margin-top: 6px;
    }
    p {
      text-align: center;
      line-height: 120px;
    }
  }
}
</style>