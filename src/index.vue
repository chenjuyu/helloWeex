<template>
  <div class="wrapper">
    <image :src="logo" class="logo" />
    <text class="greeting">The environment is ready!</text>
    <input  class="testInput" type="text"    placeholder="写的什么东西看一下"   value="aaa" />
    <router-view/>

      <div class="content">
      <input  class="testInput" type="text" :autofocus="true"    placeholder="写的什么东西看一下吧" v-model="username" /> </div>
    <wxc-button text="确定"
              @wxcButtonClicked="wxcButtonClicked"></wxc-button>
    <list>
    <cell v-for="num in lists">
      <text>{{num.Code}}</text>
       <text>{{num.Name}}</text>
    </cell>
   </list>


  </div>
 
</template>

<script>
import { WxcButton } from 'weex-ui'
const stream = weex.requireModule('stream');
const modal=weex.requireModule('modal');
const res= weex.requireModule('myModule');
//let lists=[];
export default {
  name: 'App',
  data () {
    return {
     // logo: 'https://gw.alicdn.com/tfs/TB1yopEdgoQMeJjy1XaXXcSsFXa-640-302.png',
     logo:'http://www.fuxi.com/images/home_logo.png',
     username:'',
     lists: [], //'A', 'B', 'C', 'D', 'E'
   //  array:new Array(),
     postResult:'loading...'

    }
  },
  created:function() {
    var me=this;
    var POST_URL = 'http://192.168.1.25:8080/testmybatis/syndata.do?syntools&currpage=1&pagesize=20&field=EmployeeID,Code,Name&tbl=Employee&keyid=EmployeeID';
   // const body = JSON.stringify({title, article});
   /*     
        stream.fetch({
          method: 'GET',
          url: POST_URL,
          type:'json',
          headers:{'Content-Type':'application/json'}
         // body: body
        }, function(ret) {
          //self.loading = false;JSON.stringify(ret.obj)
          console.log("数据："+ret.obj);
          if(!ret.ok){
            modal.toast({
              message: 'Network Error!',
              duration: 3
            });
          }else{
            modal.toast({
              message: '提交成功!', //+ body,
              duration: 3
            });
          }
        }) */
              //function(event)
       res.openWx("给android的模块传送",(event)=>{
                    //回调后处理 
                  // func.printLog('回调: ' + event);
                  console.log("回调的值aaaa:"+event);
                   console.log("回调的值msg:"+event.msg);
                 console.log("回调的值 obj:"+event.obj);
             
                   var a=[];//new Array();
                    a=event.obj;
                   for(var i=0;i<a.length;++i){
                    console.log("编码："+a[i].Code);
                    console.log("名字："+a[i].Name);
                   }
                console.log("this的值："+this);
                this.lists=a;//JSON.stringify(event.obj);

                //Array a=event.obj;
                // var array=new Array();
                  //array=event.obj;
                /*  for(var i=0;i<array.Length;i++){
                    console.log("编码："+array[i].Code);
                    console.log("名字："+array[i].Name);
                  }   */



                });


  },
   components: { WxcButton },
    methods: {
      wxcButtonClicked (e) {
        console.log(e)
        console.log(this.username)
      }
    }
}
</script>

<style scoped>
  .content {
    justify-content: center;
    align-items: center;
  }

  .wrapper {
    justify-content: center;
    align-items: center;
  }

  .logo {
    width: 100px;
    height: 100px;
  }
  .greeting {
    text-align: center;
    margin-top: 70px;
    font-size: 50px;
    color: #41B883;
  }
  .message {
    margin: 30px;
    font-size: 32px;
    color: #727272;
  }
  .testInput{
    height:80px; 
    padding:10px;
    font-size:32px; 
    width:750px;
    border-bottom-width:1px;
    }
</style>
