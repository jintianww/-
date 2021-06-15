<template>
  <div class="myheader">
    {{title}}
  </div>
</template>

<script>
  export default {
    props: {
      title: {
        type: String,
        default: '手机号登录'
      },
    },
  }
</script>

<style lang="scss" scoped>
.myheader{
  background-color: #8A2BE1;
  height: 100%;
  line-height: 50px;
  height: 50px;
  font-weight: bold;
  font-size: 18px;
  text-align: center;
}
</style>