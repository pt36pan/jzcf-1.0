<template>

  <div class="dialog-window" @click="closeDialog($event,-1)">
    <div class="shadow"></div>
    <transition name="scale">
      <div class="dialog-content" v-show="isShow">
        <slot name="head"></slot>
        <slot name="body">
          <!-- body-row -->
          <div class="dialog-content-bodyRow" v-if="dialogType===0">
            <div v-for="(item,key) in mData" :key="key" class="dialog-row">
              <span class="dialog-row-title">{{item.keyCN}}:</span>
              <input type="text" class="dialog-row-input" v-model="item.value"/>
            </div>
          </div>

          <!-- body-col -->
          <div class="dialog-content-bodyCol" v-if="dialogType===1">
            <transition-group name="fade">
              <div class="dialog-col" v-for="(item,key) in items" :key="item.id" v-show="key===colPos">
                <div class="dialog-col-item">
                  <span>时间</span>
                  <div>{{item.time}}</div>
                </div>
                <div class="dialog-col-item">
                  <span>地点</span>
                  <div>{{item.address}}</div>
                </div>
                <div class="dialog-col-item">
                  <span>违章行为</span>
                  <div>{{item.content}}</div>
                </div>
                <div class="dialog-col-item">
                  <span>罚款金额</span>
                  <div>{{item.price}}</div>
                </div>
                <div class="dialog-col-item">
                  <span>扣除分数</span>
                  <div>{{item.score}}</div>
                </div>
              </div>
            </transition-group>
            <div class="dialog-col-nav nav-left" @click="change(0)">{{'<'}}</div>
            <div class="dialog-col-nav nav-right" @click="change(1)">{{'>'}}</div>
            <div class="dialog-col-nav nav-bottom">当前是第{{colPos + 1}}条违章记录</div>
          </div>


        </slot>
        <slot name="foot">
          <div class="dialog-content-footRow">
            <div v-if="dialogType===0" class="dialog-button dialog-button-cancel" @click="closeDialog($event,0)">取消</div>
            <div v-if="dialogType===0" class="dialog-button dialog-button-select" @click="getData">查询</div>
            <div v-if="dialogType===1" class="dialog-button dialog-button-cancel" @click="closeDialog($event,1)">关闭</div>
          </div>

        </slot>
      </div>
    </transition>
  </div>

</template>

<script type="text/ecmascript-6">
  export default{
    components: {},
    props: {
      items: {
        type: Array,
        default: []
      },
      dialogType: {
        type: Number,
        default: -1
      },
      isShow: {
        type: Boolean,
        default: false
      },
      //  0-row  1-col
      dialogType: {
        type: Number,
        default: 0
      }
    },
    computed: {
      mData: function () {
        //      深拷贝对象 避免修改mData影响到父级props过来的数据，此方法只能对简单的对象进行深拷贝
        let obj = {};
        obj = JSON.parse(JSON.stringify(this.items)); //this.templateData是父组件传递的对象
        return obj
//          this.mData = obj;
      }
    },
    data(){
      return {
//          col类
        colPos: 0
//          row类
      }
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
      closeDialog(event, type){
        let className = event.srcElement.className;
        if(className === 'shadow' || className === 'dialog-content' || className === 'dialog-button dialog-button-cancel'){
          let data = {
            type: type
          };
          this.$emit('close-dialog', data);
        }else{
            console.log('xxx');
        }
      },
      getData(){
        let params = {};
        let _this = this;
        let items = _this.mData;
        for (let i = 0; i < items.length; i++) {
          params[items[i].key] = items[i].value;
        }
        let data = {
          params: params,
          dialogType: _this.dialogType
        };
        this.$emit('dialog-confirm', data);
      },
      change(direction){  // 0-向左 1-向右
        let position = this.colPos;
        if (direction === 0) {
          console.log('before:' + position);
          position = (position - 1) < 0 ? (this.items.length - 1) : (position - 1);
          console.log(position);
        } else {
          console.log('before:' + position);
          position = (position + 1) >= this.items.length ? 0 : (position + 1);
          console.log(position);
        }
        this.colPos = position;
        position = null;
      }
    }

  }
</script>
<style scoped lang="scss">
  .dialog-window {
    width: 100%;
    height: 100%;
    position: relative;
  }

  .shadow {
    position: absolute;
    width: 100%;
    height: 100%;
    background-color: gray;
    opacity: .7;
  }

  .dialog-content {
    padding: 48px;
    background-color: transparent;
    position: relative;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
  }

  /*body*/
  .dialog-content-bodyRow {
    width: 100%;
    background-color: white;
    overflow: hidden;
  }

  .dialog-row {
    width: 90%;
    height: 36px;
    margin: 5px 0 5px 5%;
  }

  .dialog-row-title {
    min-width: 20%;
    height: 36px;
    line-height: 36px;
    font-size: 14px;
    float: left;
    text-align: left;
    &:before {
      content: '*';
      color: red;
      width: 14px;
      height: 14px;
      line-height: 14px;
    }
  }

  .dialog-row-input {
    display: inline-block;
    width: 40%;
    height: 28px;
    line-height: 28px;
    padding: 0;
    margin-top: 3px;
    border: solid 1px lightgray;
    float: right;
  }

  /*body-col*/

  .dialog-content-bodyCol {
    width: 100%;
    height: 400px;
    background-color: white;
    position: relative;
  }

  .dialog-col {
    width: 70%;
    left: 15%;
    top: 8px;
    height: 384px;
    margin: auto;
    text-align: center;
    border-top: solid 1px lightgray;
    border-bottom: solid 1px lightgray;
    position: absolute;
    &-item {
      margin: 12px auto;
      & span {
        display: block;
        font-weight: bold;
        font-size: 16px;
      }
      & div {
        font-size: 14px;
      }
    }
  }

  .dialog-col-nav {
    height: 384px;
    line-height: 384px;
    font-size: 18px;
    position: absolute;
    transform: translateY(-50%);
    font-weight: bold;
    &.nav-left {
      width: 24px;
      top: 50%;
      left: 8px;
    }
    &.nav-right {
      width: 24px;
      top: 50%;
      right: 8px;
    }
    &.nav-bottom {
      width: 100%;
      bottom: 0;
      left: 50%;
      transform: translate(-50%, -50%);
      height: 18px;
      line-height: 18px;
      font-size: 14px;
    }
  }

  /*body-col END*/

  /*body END*/

  /*foot*/

  .dialog-content-footRow {
    background-color: white;
    width: 100%;
    height: 48px;
    display: flex;
    justify-content: space-around;
    align-items: center;
  }

  /*foot END*/

  .dialog-button {
    width: 96px;
    height: 36px;
    line-height: 36px;
    font-size: 18px;
    text-align: center;
    color: white;
  }

  .dialog-button-cancel {
    background-color: red;
  }

  .dialog-button-select {
    background-color: dodgerblue;
  }

  /*动画*/
  /*v-enter-active 和 v-leave-active 可以控制进入/离开过渡的不同的缓和曲线，在*/
  .fade-enter-active, .fade-leave-active {
    transition: all .5s;
  }

  .fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */
  {
    opacity: 0;

  }

  .fade-leave-to {
    top: 200px;
  }

  .fade-enter-to, .fade-leave {
    opacity: .7;
  }

  .scale-enter-active, .scale-leave-active {
    transition: all .3s;
  }

  .scale-enter, .scale-leave-to {
    left: 0;
    top: 0;
    transform: scale(0) translate(-50%, -50%);
  }

  .scale-enter-to, .scale-leave {
    left: 50%;
    top: 50%;
    transform: scale(1) translate(-50%, -50%);
  }

</style>
