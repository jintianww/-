<template>
  <button @click="cls">{{tit}}</button>
</template>

<script>
import { ref } from "vue";
export default {
  setup(pop,cnt) {
    let tit = ref("发送验证码");
    console.log(cnt.attrs.tel);
    function cls(){
        let hom=60;
        if(tit.value=='发送验证码'){
            if(/^[0-9]{11}$/.test(cnt.attrs.tel)){
                // alert('发送成功')
                console.log(cnt.attrs.tel);
                        tit.value='倒计时(60)';
                    setInterval(()=>{
                        if(hom<=1){
                            tit.value='发送验证码';
                        }else{
                            hom--;
                            tit.value='倒计时('+hom+')'
                        }
                    },1000)
            }
        }
    }
    return {
      tit,
      cls,
    };
  },
};
</script>

<style lang="scss" scoped>
</style>