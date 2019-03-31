<template>
  <div class="container">
      <head :title="tit" @titleClick="clickT" :height="120" @rightClick="clickR">
          <text  slot="right" style="flex-direction: row;margin-top: 13px;">右边</text>
      </head>

      <!-- weex-ui  :sourceData="sourceData" class="scroller"  :sourceData="sourceData"城市录入方式 showListCity showyuangong-->
      <scroller class="scroller" >
              <div style="height: 102px;width: 750px"><input type="text" class="input" return-key-type="default" placeholder="default" v-model="no"></div>
              <div class="btn" @click="showyuangong">
                  <text class="btn-txt">城市选择</text>
              </div>
              <div class="panel">
                  <text v-if="currentCity" class="text">当前城市: {{currentCity}}</text>
                  <text>B页面返回的值：{{value}}</text>
              </div>

          </scroller>

      <basecheck ref="cjy" @choice="submit"></basecheck>



      <wxc-city ref="wxcCity"
                :animationType="animationType"
                :currentLocation="location"
                :cityStyleType="cityStyleType"

                :showNavHeader="true"
                @wxcCityItemSelected="citySelect"
                @wxcCityOnInput="onInput"></wxc-city>


    <scroller :style="{'width':width+'px'}" scroll-direction='horizontal'>
      <div class="label">
        <text class="item">单号</text>
        <text class="item">日期</text>
        <text class="item">制单人</text>
        <text class="item">审核人</text>
        <text class="item">审核时间</text>
        <text class="item">订单2</text>
        <text class="item">tt</text>
      </div>
      <scroller :style="{'width':width+'px'}">
      <div class="cell" v-for="(num,key) in lists" @click="onSelected(num,key)" >
        <text class="item">{{num.No}}</text>
       <text class="item">{{num.Date}}</text>
        <text class="item">{{num.madeby}}</text>
        <text class="item">{{num.audit}}</text>
         <text class="item">{{num.Date}}</text>
        <text class="item">{{num.order}}</text>
        <text class="item">{{num.tt}}</text>
      </div>
    </scroller>
    </scroller>







  </div>
</template>

<script>
    import { WxcCity,Utils  } from 'weex-ui';
    import basecheck  from './basecheck.vue';
  const dom = weex.requireModule('dom')
  const test=weex.requireModule('test');
  let array={}
  export default {
      components: { basecheck,WxcCity

      },
    data () {
      return {
          animationType: 'push',
          currentCity: '',
           no:'',
          cityStyleType:'list',
          location: '定位中',
        lists: [{'No':123,'Date':'2019-03-03','madeby':'系统管理员','audit':'陈陈陈','auditDate':'2019-03-03','order':'adafdsfsdfa','tt':'sdsdfsdfsdf'},
          {'No':456,'Date':'2019-03-04','madeby':'系统管理员1','audit':'陈陈陈','auditDate':'2019-03-03','order':'adafdsfsdfa','tt':'sdsdfsdfsdf'},
          {'No':789,'Date':'2019-03-05','madeby':'系统管理员2','audit':'陈陈陈','auditDate':'2019-03-03','order':'adafdsfsdfa','tt':'sdsdfsdfsdf'},
          {'No':789,'Date':'2019-03-05','madeby':'系统管理员2','audit':'陈陈陈','auditDate':'2019-03-03','order':'adafdsfsdfa','tt':'sdsdfsdfsdf'},
          {'No':111,'Date':'2019-03-05','madeby':'系统管理员2','audit':'陈陈陈','auditDate':'2019-03-03','order':'adafdsfsdfa','tt':'sdsdfsdfsdf'},
          {'No':222,'Date':'2019-03-05','madeby':'系统管理员2','audit':'陈陈陈','auditDate':'2019-03-03','order':'adafdsfsdfa','tt':'sdsdfsdfsdf'},
          {'No':333,'Date':'2019-03-05','madeby':'系统管理员2','audit':'陈陈陈','auditDate':'2019-03-03','order':'adafdsfsdfa','tt':'sdsdfsdfsdf'},
          {'No':444,'Date':'2019-03-05','madeby':'系统管理员2','audit':'陈陈陈','auditDate':'2019-03-03','order':'adafdsfsdfa','tt':'sdsdfsdfsdf'},
          {'No':123,'Date':'2019-03-03','madeby':'系统管理员','audit':'陈陈陈','auditDate':'2019-03-03','order':'adafdsfsdfa','tt':'sdsdfsdfsdf'},
          {'No':456,'Date':'2019-03-04','madeby':'系统管理员1','audit':'陈陈陈','auditDate':'2019-03-03','order':'adafdsfsdfa','tt':'sdsdfsdfsdf'},
          {'No':789,'Date':'2019-03-05','madeby':'系统管理员2','audit':'陈陈陈','auditDate':'2019-03-03','order':'adafdsfsdfa','tt':'sdsdfsdfsdf'},
          {'No':789,'Date':'2019-03-05','madeby':'系统管理员2','audit':'陈陈陈','auditDate':'2019-03-03','order':'adafdsfsdfa','tt':'sdsdfsdfsdf'},
          {'No':111,'Date':'2019-03-05','madeby':'系统管理员2','audit':'陈陈陈','auditDate':'2019-03-03','order':'adafdsfsdfa','tt':'sdsdfsdfsdf'},
          {'No':222,'Date':'2019-03-05','madeby':'系统管理员2','audit':'陈陈陈','auditDate':'2019-03-03','order':'adafdsfsdfa','tt':'sdsdfsdfsdf'},
          {'No':333,'Date':'2019-03-05','madeby':'系统管理员2','audit':'陈陈陈','auditDate':'2019-03-03','order':'adafdsfsdfa','tt':'sdsdfsdfsdf'},
          {'No':444,'Date':'2019-03-05','madeby':'系统管理员2','audit':'陈陈陈','auditDate':'2019-03-03','order':'adafdsfsdfa','tt':'sdsdfsdfsdf'},
          {'No':123,'Date':'2019-03-03','madeby':'系统管理员','audit':'陈陈陈','auditDate':'2019-03-03','order':'adafdsfsdfa','tt':'sdsdfsdfsdf'},
          {'No':456,'Date':'2019-03-04','madeby':'系统管理员1','audit':'陈陈陈','auditDate':'2019-03-03','order':'adafdsfsdfa','tt':'sdsdfsdfsdf'},
          {'No':789,'Date':'2019-03-05','madeby':'系统管理员2','audit':'陈陈陈','auditDate':'2019-03-03','order':'adafdsfsdfa','tt':'sdsdfsdfsdf'},
          {'No':789,'Date':'2019-03-05','madeby':'系统管理员2','audit':'陈陈陈','auditDate':'2019-03-03','order':'adafdsfsdfa','tt':'sdsdfsdfsdf'},
          {'No':111,'Date':'2019-03-05','madeby':'系统管理员2','audit':'陈陈陈','auditDate':'2019-03-03','order':'adafdsfsdfa','tt':'sdsdfsdfsdf'},
          {'No':222,'Date':'2019-03-05','madeby':'系统管理员2','audit':'陈陈陈','auditDate':'2019-03-03','order':'adafdsfsdfa','tt':'sdsdfsdfsdf'},
          {'No':333,'Date':'2019-03-05','madeby':'系统管理员2','audit':'陈陈陈','auditDate':'2019-03-03','order':'adafdsfsdfa','tt':'sdsdfsdfsdf'},
          {'No':444,'Date':'2019-03-05','madeby':'系统管理员2','audit':'陈陈陈','auditDate':'2019-03-03','order':'adafdsfsdfa','tt':'sdsdfsdfsdf'},
          {'No':123,'Date':'2019-03-03','madeby':'系统管理员','audit':'陈陈陈','auditDate':'2019-03-03','order':'adafdsfsdfa','tt':'sdsdfsdfsdf'},
          {'No':456,'Date':'2019-03-04','madeby':'系统管理员1','audit':'陈陈陈','auditDate':'2019-03-03','order':'adafdsfsdfa','tt':'sdsdfsdfsdf'},
          {'No':789,'Date':'2019-03-05','madeby':'系统管理员2','audit':'陈陈陈','auditDate':'2019-03-03','order':'adafdsfsdfa','tt':'sdsdfsdfsdf'},
          {'No':789,'Date':'2019-03-05','madeby':'系统管理员2','audit':'陈陈陈','auditDate':'2019-03-03','order':'adafdsfsdfa','tt':'sdsdfsdfsdf'},
          {'No':111,'Date':'2019-03-05','madeby':'系统管理员2','audit':'陈陈陈','auditDate':'2019-03-03','order':'adafdsfsdfa','tt':'sdsdfsdfsdf'},
          {'No':222,'Date':'2019-03-05','madeby':'系统管理员2','audit':'陈陈陈','auditDate':'2019-03-03','order':'adafdsfsdfa','tt':'sdsdfsdfsdf'},
          {'No':333,'Date':'2019-03-05','madeby':'系统管理员2','audit':'陈陈陈','auditDate':'2019-03-03','order':'adafdsfsdfa','tt':'sdsdfsdfsdf'},
          {'No':444,'Date':'2019-03-05','madeby':'系统管理员2','audit':'陈陈陈','auditDate':'2019-03-03','order':'adafdsfsdfa','tt':'sdsdfsdfsdf'}

          ]
      }
      },props: {
      tit:{
        default: '列表页aaaaa'
      },
      width:{
        default:1400
      },
          sourceData:{

              default:array
          },value:{
          default:''
          }
    },methods:{
          clickT(){
              this.toast('点击标题栏')
          },clickR(){
              this.toast('点击右边')
          },
          onSelected(num,key){
              this.alert(num.No);
              this.push("root:possalesdetail.js",{"No":num.No,"madeby":num.madeby});

          }, showListCity () {

              this.sourceData= {hotCity:[{ cityName: '北京', pinYin: 'beijing', py: 'bj' },
                      { cityName: '包头', pinYin: 'baotou', py: 'bt' },
                      { cityName: '北海', pinYin: 'beihai', py: 'bh' },
                      { cityName: '宝鸡', pinYin: 'baoji', py: 'bj' }]};



            //  this.$refs['wxcCity'].saveDefaultSourceData =this.sourceData;
              //this.$refs['wxcCity'].saveDefaultSourceData =this.sourceData
              this.$refs['wxcCity'].sourceData=Utils.paramsToObj({hotCity:[{ cityName: '北京', pinYin: 'beijing', py: 'bj' },
                      { cityName: '包头', pinYin: 'baotou', py: 'bt' },
                      { cityName: '北海', pinYin: 'beihai', py: 'bh' },
                      { cityName: '宝鸡', pinYin: 'baoji', py: 'bj' }],
                  cities:[
                      { cityName: '北京', pinYin: 'beijing', py: 'bj'},
                      { cityName: '包头', pinYin: 'baotou', py: 'bt' }]  });
              this.$refs['wxcCity'].saveDefaultSourceData =this.sourceData
              this.cityStyleType = 'list';
            //   this.alert(this)
             // this.alert(this.$refs['wxcCity']);

             this.$refs['wxcCity'].show();
          },
          showGroupCity () {
              this.cityStyleType = 'group'
              this.$refs['wxcCity'].show();
          },
          citySelect (e) {
              this.currentCity = e.item;
          },
          onInput (e) {
          },
          showyuangong(){
              this.$refs.cjy.show()
          },
          submit(e){
              //this.toast(e.value)
             // this.alert(e)
              this.value=e.item
          }
      },
      created() {
        let pa={}
        pa.str="给android的一个请求条件";
        pa.text="aaaa";

         test.testpint(pa,(e)=>{
           debugger
           console.log("返回的内容:"+e);
         });
    },mounted () {
          // 模拟定位
          setTimeout(() => {
              this.location = '杭州';
          }, 500);
      },

  }
</script>
<style>
  .container {

  }
  .input {
      border-width: 1px;
      margin-bottom: 5px;
      margin-top: 5px;
      height: 100px;
  }
  .row {
      width: auto;
      flex: 1;
      position: relative;
    }
  .cell {
    background-color: red;
    flex-direction: row;
  }
  .item{
    width:200px;
    line-height: 80px;
  }
  .label{
    width: auto;
    flex-direction: row;
    background-color:orangered;
  }

  .scroller {
      /*  flex: 1;  这个等于1会点用半屏*/
        height: 400px;
    }
  .btn {
      width: 600px;
      height: 80px;
      margin-left: 75px;
      margin-top:100px;
      flex-direction: row;
      align-items: center;
      justify-content: center;
      border-radius: 6px;
      background-color: rgb(92, 184, 92);
      border-color: rgb(76, 174, 76);
  }
  .btn-txt {
      font-size: 32px;
      color: #ffffff;
  }
  .btn-margin {
      margin-top: 40px;
  }
  .panel {
      height: 100px;
      align-items: center;
      margin-top: 40px;

  }

</style>