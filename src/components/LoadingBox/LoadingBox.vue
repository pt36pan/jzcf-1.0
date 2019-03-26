<template>

  <div class="loadingBox" v-show="loading">
    <ul>
      <li class="later"></li>
      <li class="normal"></li>
      <li class="early"></li>
    </ul>
  </div>

</template>

<script type="text/ecmascript-6">
  export default{
    components: {},
    props: {
      loading: {
        type: Boolean,
        default: false
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
    methods: {}

  }
</script>
<style scoped lang="scss">
  .loadingBox {
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    padding: 0;
    & ul {
      width: 320px;
      padding: 0;
      & li {
        display: inline-block;
        margin: 8px;
        background: dodgerblue;
        width: 32px;
        height: 32px;
        border-radius: 50%;
        animation: loading 1s infinite ease-in-out;
      }
    }
  }

  .early {
    opacity: 1;
  }

  .later {
    opacity: .5;
  }

  .normal {
    opacity: .7;
  }

  @keyframes loading {
    0%, 100% {
      transform: scale(0.0);
    }
    50% {
      transform: scale(1);
    }
  }

</style>
