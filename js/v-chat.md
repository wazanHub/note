&emsp;&emsp;一个简单的仿群聊h5交互小页面。
>### 介绍

&emsp;&emsp;之前某祺汽车客户搞系列线上留资活动，其中有个类似机器人的仿微信群聊h5,还没最终确稿前组长叫我去研究下制作,第二天我就用jq写出来了，可以说完成了基本的功能(也没什么功能)。后来听说需求腰斩，也没上线，闲来有空用vue重构下(其实这么简单的h5似乎没必要用vue来做，这里是自己因为对vue的不熟悉，以此来练习)<br>
**_结果都一样，最重要的是过程_**<br>
参考案例是大概是小米做的一个互动h5页面。
![](https://user-gold-cdn.xitu.io/2020/5/14/172139b43f48276d?w=100&h=101&f=png&s=15202)
![](https://user-gold-cdn.xitu.io/2020/5/14/1721395b24f9d292?w=267&h=131&f=png&s=12946)

>### 需求
* 入口添加用户名或者先微信授权进入，获取微信头像，名称等信息
![](https://user-gold-cdn.xitu.io/2020/5/14/17213af392e6e9ab?w=200&h=120&f=png&s=23919)
* 进入活动页面，显示日常微信进群的样式，自动进行开场对白聊天
![](https://user-gold-cdn.xitu.io/2020/5/14/17213a623e28a5e1?w=200&h=136&f=png&s=20968)
* 首段聊天对白结束后，底下输入功能区变为若干提问的问题点，手动点击又会出现自动的对白聊天
![](https://user-gold-cdn.xitu.io/2020/5/14/17213b0a62cd7157?w=310&h=130&f=png&s=9532)
>### 开干思路
##### &emsp;&emsp;主要开发点：
* 每出一条最新对话记录使聊天页面滑到最底处
* 定时加载输出既定对白记录
##### &emsp;&emsp;注意点：
* 每段对白结束前不可点击下面的提问按钮



>### 代码回顾
##### 1.vue-cli搭建项目
(主要要配置什么)
##### 2.确定组件结构
* App
* Main(聊天记录展示区)、Bottom(底部控制输出区)
* Record(每段对白群)
```
<!--App.vue-->
<template>
  <div id="app">
    <div class="doc" ref="doc">
      <Main ref="main"></Main>
      <Bottom ref="bottom"></Bottom>
    </div>
  </div>
</template>
```
##### 3.页面容器尺寸分配渲染
![](https://user-gold-cdn.xitu.io/2020/5/14/17213e114239d7ce?w=494&h=548&f=png&s=55387)
明显Bottom的高度无论横屏竖屏都应该是固定的，变化的是Main的高度。<br>
这里写个方法在初始化或者窗口发生变化时计算并渲染这两块容器的高度
```
frameChange(){
      this.frameInit()           
      this.scrollBottom()
      window.onresize=()=>{
        this.frameInit()           
        this.scrollBottom()
      }
    },

    //计算并渲染布局Main
    frameInit(){
      let main=this.$refs.main.$el//获取main dom
      let bottom=this.$refs.bottom.$el//获取main dom
      let mheight=window.innerHeight-bottom.offsetHeight//实时算出Main组件所占高度
      main.style.height=mheight+'px'
    },
```
在生命钩子触发
```
mounted(){
    this.frameChange()
  }
```
##### 对白数据从哪里来
Mockjs模拟接口数据<br>
根据父子组件的生命钩子确定在子组件Record的mouted里调用createData获取数据<br>
##### 使用vuex创建一个公共状态
写一个Promise执行从获取到渲染数据的过程<br>
```
createData(){
            let that=this;

            new Promise(function(resolve,reject){
                axios.get('mdata1/mdata1').then(data=>{
                resolve(data)
                }).catch(err=>{
                reject(err)
                })

            }).then(function(data){
                that.$store.commit('createData',data.data.getData.data.data)
            }).catch(err=>{
                console.log(err);
            }).then(function(){
                that.all=that.$store.state.dialogue
                that.currentRecords=that.all[0].list
            }).catch(err=>{
                console.log(err)
            }).then(function(){
                //定时输出每条聊天记录
                let i=0
                let timer=setInterval(()=>{
                    if(i===that.$refs.child.length){
                        clearInterval(timer);
                        that.$parent.$parent.$children[1].mask=false
                        return;
                    }
                    that.$refs.child[i].style.display="block"
                    i++;

                    that.$parent.$parent.scrollBottom()//执行滑动到底部
                },1500)

                
            }).catch(err=>{
                console.log(err)
            })
```












##### 重点Main组件里
```
<!--Main.vue-->
<template>
  <div class="main">
    <Record></Record>
  </div>
</template>
```























>### 项目总结


































