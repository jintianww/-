<template>
  <div class="home">
    <myheader></myheader>
    <div class="mpy">
      <div>
        <span>手机号</span>：
        <input type="text" v-model="tel" placeholder="手机号" />
      </div>
      <div>
        <span>手机号</span>：
        <input type="text" v-model="yan" placeholder="验证码" />
        <mybutton :tel="tel"></mybutton>
      </div>
      <button @click="deng" class="btn" :class="{dis:!disabled}" :disabled="disabled">登录</button>
    </div>
  </div>
</template>

<script>
import Mybutton from "../components/mybutton.vue";
import Myheader from "../components/myheader.vue";
import { ref, watchEffect } from "vue";
import { useRouter } from "vue-router";
export default {
  setup() {
    let router = useRouter();
    let tel = ref("");
    let yan = ref("");
    let disabled = ref(true);
    watchEffect(() => {
      if (
        /^[0-9]{11}$/.test(tel.value) &&
        /^[0-9a-zA-Z]{4,6}$/.test(yan.value)
      ) {
        disabled.value = false;
      } else {
        disabled.value = true;
      }
    });
    function deng() {
      router.push("/about");
      tel.value = "";
    }
    return {
      tel,
      yan,
      disabled,
      deng,
    };
  },
  components: { Myheader, Mybutton },
};
</script>

<style lang="scss" scoped>
.home {
  width: 100%;
  height: 100%;
}
.mpy {
  width: 100%;
  height: calc(100% - 50px);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  div {
    width: 340px;
    margin: 6px 0;
  }
  input {
    height: 30px;
    margin-right: 6px;
    text-indent: 12px;
  }
  button {
    height: 30px;
  }
  .btn {
    padding: 6px 24px;
  }
}

.dis {
  background-color: skyblue;
}
</style>