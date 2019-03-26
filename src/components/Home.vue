<template>

  <div class="home">
    <div class="home-content">
      <my-cube @cube-click="handleCubeClick" v-for="(item,key) in items" :key="key" :mType="item.mType"
               :title="item.title">
        <div slot="body">
          <img :src="item.img">
        </div>
      </my-cube>
    </div>
    <my-dialog v-show="showDialog" @dialog-confirm="handleDialogConfirm"
               @close-dialog="handleCloseDialog"
               :items="dialogData" :dialogType="dialogType" :isShow="showDialog">
      <div slot="head" style="height: 48px;line-height: 48px;font-weight: bold;background: white">
        {{dataType === 0 ? '车辆违章查询' : '个人驾照查分'}}
      </div>
    </my-dialog>

    <!--查询结果弹窗-->
    <my-dialog v-show="showResult"
               @close-dialog="handleCloseDialog"
               :isShow="showResult" :items="resultData" :dialogType="1">
      <div slot="head" style="height: 48px;line-height: 48px;font-weight: bold;background: white"
           v-show="dialogType === 1">
        {{dataType === 0 ? '违章记录详情' : '驾照分数详情'}}
      </div>
    </my-dialog>

    <my-loading-box :loading="loading"></my-loading-box>
  </div>

</template>

<script type="text/ecmascript-6">

  //components
  import MyCube from './Cube/Cube.vue';
  import MyDialog from './Dialog/Dialog.vue';
  import MyLoadingBox from './LoadingBox/LoadingBox.vue';
  //  img
  import carImg from '../assets/car.png';
  import idCardImg from '../assets/idcard.png';

  export default{
    components: {
      MyCube,
      MyDialog,
      MyLoadingBox
    },
    props: {},
    computed: {
      dialogData: function () {
        return this.dataType === 0 ? this.dialogItemsCar : this.dialogItemsMan
      }
    },
    data(){
      return {
        items: [
          {title: '车辆违章查询', img: carImg, mType: 0},
          {title: '驾照扣分查询', img: idCardImg, mType: 1}
        ],
        showDialog: false,
        showResult: false,
        dialogItemsCar: [
          {
            id: '01',
            key: 'carorg',
            keyCN: '省份(如：guangxi)',
            value: 'guangxi'
          },
          {
            id: '02',
            key: 'lsprefix',
            keyCN: '城市（如：桂）',
            value: '桂'
          },
          {
            id: '03',
            key: 'lsnum',
            keyCN: '车牌号',
            value: 'BYH621'
          },
          {
            id: '04',
            key: 'lstype',
            keyCN: '类型（小车:02）',
            value: '02'
          },
          {
            id: '05',
            key: 'engineno',
            keyCN: '发动机号6位',
            value: '896109'
          }
        ],
        dialogItemsMan: [
          {
            id: '01',
            key: 'cardId',
            keyCN: '驾驶证号',
            value: ''
          }
        ],
//        弹窗的类型 0-cube弹窗 1-查询结果弹窗
        dialogType: 0,
//        数据类型 0-违章查询 1-驾照查询
        dataType: 0,
//        查询结果：
        resultData: [
          {
            "time": "2015-06-23 18:24:00.0",
            "address": "赵非公路鼓浪路北约20米",
            "content": "违反规定停放、临时停车且驾驶人不在现场或驾驶人虽在现场拒绝立即驶离，妨碍其他车辆、行人通行的",
            "legalnum": "",
            "price": "0",
            "id": "3500713",
            "score": "0"
          },
          {
            "time": "2015-06-05 18:20:00.0",
            "address": "新松江路近人民北路东侧路段",
            "content": "违反规定停放、临时停车且驾驶人不在现场或驾驶人虽在现场拒绝立即驶离，妨碍其他车辆、行人通行的",
            "legalnum": "",
            "price": "0",
            "id": "3500714",
            "score": "0"
          },
          {
            "time": "2015-06-08 18:22:00.0",
            "address": "鼓浪路近291弄路段",
            "content": "违反规定停放、临时停车且驾驶人不在现场或驾驶人虽在现场拒绝立即驶离，妨碍其他车辆、行人通行的",
            "legalnum": "",
            "price": "0",
            "id": "3500715",
            "score": "0"
          }
        ],
        loading: false
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
      handleCubeClick(data){
        let type = data.mType;
        this.dataType = type;
        this.showDialog = true;
      },
      handleDialogConfirm(data){
        if (data.dialogType === 0) {
          console.log('关闭弹窗');
          let params = data.params;
          let url = 'https://api.jisuapi.com/illegal/query?appkey=e8fe74557b878da8&carorg=' + params.carorg + '&lsprefix=' + params.lsprefix + '&lsnum=' + params.lsnum + '&lstype=' + params.lstype + '&frameno=6&engineno=' + params.engineno + '&iscity=1'
          let _this = this;
          _this.loading = true;
          _this.$http.get(url)
            .then(function (response) {
              alert(response);
              _this.loading = false;
              _this.showDialog = false;
              _this.showResult = true;
              _this.dialogType = 1;
            })
            .catch(function (error) {
              _this.showDialog = false;
              _this.loading = false;
              alert(error)
            });
        } else if (data.dialogType === -1) {
          console.log('即将开放');
        }
      },
      handleCloseDialog(data){
        let _this = this;
        if (data.type === 0 || data.type === -1) {
          _this.showDialog = false;
        } else if (data.type === 1) {
          _this.showResult = false;
        }
        _this.dialogType = 0;
      }
    }

  }
</script>
<style scoped lang="scss">
  .home {
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
    position: absolute;
  }

  .home-content {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
  }

  img {
    width: 72px;
  }

</style>
