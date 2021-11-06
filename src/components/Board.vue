<template>
  <div id="board">
      <div id="header">
          <div class="title">Message Board</div>
          <div class="option" ref="option" @click="optionClick">
          </div>
      </div>
      <div id="messages" class="content content_loading" ref="content">
          <div class="scrollBox">
            <div class="item" v-for="(item,index) in messages" :key="index">
                <div class="avater"></div>
                <div class="itemContent">
                    <div class="name">{{item.name}}</div>
                    <div class="text">{{item.text}}</div>
                    <div class="star">
                        <img src="../assets/星星.svg" width="20" height="20" v-show="!item.isStar"/>
                        <img src="../assets/星星激活.svg" width="20" height="20" v-show="item.isStar"/>
                    </div>
                    <div class="time">{{timeFormater(item.time)}}</div>
                </div>
            </div>
          </div>
      </div>
      <div class="skeleton" ref="skeleton">
          <div class="skeleton_container">
          <div class="skeleton_item" v-for="i in 5" :key="i">
              <div class="skeleton_avater"></div>
              <div class="skeleton_name"></div>
              <div class="skeleton_text"></div>
          </div>
          </div>

      </div>
      <div class="option_container">
          <div :class="optionBoxClass">
      </div>
      </div>

      <div id="post">
          <textarea></textarea>
          <button><img src="../assets/发送.svg" width="30" height="30"></button>
      </div>
      <div id="login" @click="login">
          登录
      </div>
  </div>
</template>

<script>
import supabase from '../ultis/supabase'
import BetterScroll from 'better-scroll'
export default {
    name:'Board',
    data(){
        return{
            messages:[],
            optionBoxClass: {
                'option_box':true,
                'option_box_active':false
            },
            avaters:['1.jpg','2.jpg','3.jpg','4.jpg','5.jpg','6.jpg','7.jpg','8.jpg']
        }
    },
    methods:{
        login:async function(){
            const { user, session, error } = await supabase.auth.signIn({
                provider: 'github'
            });
        },
        getData:async function(){
            const data = await supabase.from('message').select()
            this.messages = data.data
            console.log(data.data)
        },
        timeFormater(value){
            return value.split('-')[1]+'/'+value.split('-')[2].slice(0,2)
        },
        scroll(){
            let warpper = document.querySelector('.content')
            let bs = new BetterScroll(warpper,{
                    mouseWheel: true,
                    momentum: false,
                    bounce: false,
                    scrollbar: true
                })
            console.log(bs)
        },
        optionClick(){
            this.optionClickTimes++
            this.optionBoxClass.option_box_active = true
            
        },
        loaded(){
            this.$refs.skeleton.classList.add('skeleton_loaded')
            this.$refs.content.classList.remove('content_loading')
        }
    },
    mounted(){
        this.getData()
        .then(this.scroll)
        .then(this.loaded)
    }
}
</script>

<style scoped>

#board{
    margin-top: 10px;
    width: 380px;
    border-radius: 10px;
    position: relative;
    /* border: 2px solid var(--line_color); */
}
#header{
    height: 100px;
    width: 100%;
    position: relative;
    background-color: var(--item_color);
    border-radius: 30px 30px 0 0;
    border-bottom: 1px solid var(--line_color);
    user-select: none;
}
.option{
    width: 50px;
    height: 50px;
    background-color: rgb(236, 114, 14);
    border-radius: 50%;
    position: absolute;
    left: 80%;
    top: 22px;
    cursor: pointer;
    background-image: url('../assets/Avater/1.jpg');
    background-size: cover;
}
.option_container{
    height: 200px;
    width: 100%;
    position: absolute;
    top: 362px;
    overflow: hidden;
    display: flex;
    align-items: center;
}
.option_box{
    background-color: rgb(222, 224, 224);
    height: 200px;
    width: 100%;
    position: absolute;
    top: 200px;
    transition: 0.3s ease-in-out;
    z-index: 11;
    border-radius: 12px 12px 0 0;
}
.option_box_active{
    top: 0px;
}
.title{
    position: absolute;
    left: 30px;
    top: 30%;
    font-size: 30px;
}
.skeleton{
    position: absolute;
    width: 100%;
    height: 460px;
    background-color: #fff;
    top: 100px;
}
.skeleton_container{
    height: 100%;
    display: flex;
    flex-wrap: wrap;
}
.skeleton_item{
    height: 20%;
    width: 100%;
    border-bottom: 1px solid rgb(168, 164, 164);
    position: relative;
}
.skeleton_avater{
    height: 40px;
    width: 40px;
    border-radius: 50%;
    background-color: grey;
    position: relative;
    top: 10px;
    left: 4px;
    background-size: cover;
    animation: loading ease-in-out  1.6s infinite;
}
.skeleton_name{
    position: absolute;
    left: 56px;
    top: 20px;
    background-color: grey;
    width: 70px;
    height: 18px;
    animation: loading ease-in-out  1.6s infinite;
    border-radius: 12px;

}
.skeleton_text{
    position: absolute;
    left: 54px;
    top: 46px;
    height: 40px;
    width: 400px;
    background-color: grey;
    animation: loading ease-in-out  1.6s infinite;
    border-radius: 20px;

}
@keyframes loading {
    0% {
        background-color: grey;
    }
    50% {
        background-color: rgb(168, 164, 164);
    }
    100% {
        background-color: grey;
    }
}
.content{
    width: 100%;
    margin:0 auto;
    height: 460px;
    overflow: hidden;
    position: relative;
    background-color: #fff;
    z-index: 10;
}
.content_loading{
    /* display: none; */
    opacity: 0;
}
.skeleton_loaded{
    display: none;
}
.item{
    background-color: var(--item_color);
    border-bottom: 1px solid var(--line_color);
    width: 100%;
    user-select: none;
    display: flex;
}
.item:hover{
    background-color: var(--hover_color);
}
.avater{
    height: 40px;
    width: 40px;
    border-radius: 50%;
    background-color: rgb(163, 156, 151);
    position: relative;
    top: 10px;
    left: 6px;
    background-size: cover;
}

.star{
    top: 10px;
    left: 92%;
    position: absolute;
    cursor: pointer;
}
.star img{
    position: absolute;
}
.time{
    height: 10px;
    /* width: 64px; */
    font-size: 6px;
    bottom: 10%;
    left: 90%;
    position: absolute;
    font-size: 12px;
    color: var(--text_color);
    opacity: 0.7;
}
.itemContent{
    width: 100%;
    position: relative;
}

.name{
    margin-left: 4%;
    margin-top: 14px;
    font-size: 18px;
    font-weight: 500;
}
.text{
    width: 80%;
    margin-left: 4%;
    margin-top: 14px;
    margin-bottom: 14px;
    color: var(--text_color);
    /* 溢出换行 */
    table-layout:fixed; 
    word-break:break-all; 
    overflow:hidden; 
    text-align: justify;
    opacity: 0.7;
}
#post{
    height: 150px;
    background-color: var(--item_color);
    border-radius: 0 0 30px 30px;
    position: relative;
}
#post textarea{
    position: absolute;
    top: 30%;
    left: 50%;
    transform: translate(-50%,-50%);
    resize: none;
    outline: none;
    border: 1px solid #a5bcf5;
    height: 60px;
    width: 92%;
    font-size: 22px;
}
#post button{
    position: absolute;
    outline: none;
    border: 0px solid #a5bcf5;
    border-radius: 50%;
    margin: 0;
    padding: 0;
    background-color: var(--button_color);
    font-size: 26px;
    color: #88a9fc;
    cursor: pointer;
    left: 50%;
    top: 80%;
    transform: translate(-50%,-50%);
    height: 56px;
    width: 56px;
}
#post button img{
    opacity: 0.8;
}
#post button:hover{
    background-color: #ffd3b6;
}
@media screen and (min-width: 1200px){
    #board{
        width: 500px;
    }
    .option{

        left: 88%;
    }
    .star{
        left: 94%;
    }
}
</style>