<template>

  <div class="content" @click="clickHandle">
    <slot name="head">
      <div class="title">{{title}}</div>
    </slot>
    <slot name="body">
      <div class="body">默认body</div>
    </slot>
    <div class="border"></div>
    <div class="border later"></div>

    <!--<slot name="foot">-->
    <!--<div class="foot">默认foot</div>-->
    <!--</slot>-->
  </div>

</template>

<script type="text/ecmascript-6">
  export default{
    components: {},
    props: {
      title: {
        type: String,
        default: '默认title'
      },
      mType: {
        type: Number,
        default: -1
      }
    },
    computed: {},
    data(){
      return {}
    },
    created: function () {
      //  实例创建完成后调用，此阶段完成了数据的观测等，但尚未挂载，还不可用。需要初始化处理一些数据时会比较有用。

    },
    mounted(){
      //  el挂载到实例上后会调用，第一个业务逻辑一般从这里开始。
    },
    beforeDestroy: function () {
      //  实例销毁之前调用。主要是解绑一些使用addEventListener监听的事件等。
    },
    methods: {
      clickHandle: function () {
        let data = {
          mType: this.mType
        };
        this.$emit('cube-click', data)
      }
    }

  }
</script>
<style scoped lang="scss">
  $color-main: #55CCFF;
  $screen-width: 375;
  $basic-width: $screen-width/2;
  .content {
    width: $basic-width + px;
    height: $basic-width + px;
    background-color: white;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 14px;
    margin: 14px;
  }

  .title {
    text-align: center;
    height: 24px;
    margin: 16px;
  }

  .border {
    height: $basic-width + px;
    width: $basic-width + px;
    position: absolute;
    opacity: 0;
    animation: borderRun 6s linear infinite;
    transition: all .2s;
    &.later{
      animation-delay: -3s;
    }
    &:hover{
      background: lightgray;
    }
  }

  @keyframes borderRun {
    0%, 100% {
      box-shadow: inset deepskyblue 0 0 0 2px;
      opacity: .7;
      clip: rect(0px, $basic-width+px, 2px, 0px);
    }
    25% {
      box-shadow: inset dodgerblue 0 0 0 2px;
      clip: rect(0px, 2px, $basic-width+px, 0px);
    }
    50% {
      box-shadow: inset royalblue 0 0 0 2px;
      clip: rect(($basic-width)-2+px, $basic-width+px, $basic-width+px, 0px);
    }
    75% {
      box-shadow: inset blue 0 0 0 2px;
      clip: rect(0px, $basic-width+px, $basic-width+px, ($basic-width)-2+px);
    }
  }
</style>
