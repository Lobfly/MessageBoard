<template>
  <div id="board">
      <div id="header">
          <div class="title">Message Board</div>
          <div class="option" ref="option">
          </div>
      </div>
      <div id="messages" class="content">
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
      <!-- <div class="option_container">
      <div class="option_box" ref="optionBox">
      </div>
      </div> -->

      <div id="post">
          <textarea></textarea>
          <button><img src="../assets/发送.svg" width="30" height="30"></button>
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
            messages:[]
        }
    },
    methods:{
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

        }
    },
    mounted(){
        this.getData()
        .then(this.scroll)
    }
}
</script>

<style scoped>

#board{
    margin-top: 10px;
    width: 500px;
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
    left: 88%;
    top: 22px;
    cursor: pointer;
    background-image: url('../assets/Avatar/1.jpg');
    background-size: cover;
}
.option_container{
    height: 100px;
    border: 1px solid red;
}
.option_box{
    width: 100%;
    background-color: grey;
    height: 180px;
    position: absolute;
    top: 562px;
    transition: 0.3s ease-in-out;
}
.option_box_active{
    width: 100%;
    background-color: grey;
    height: 180px;
    position: absolute;
    transition: 0.3s ease-in-out;  
    top: 400px;
}
.title{
    position: absolute;
    left: 30px;
    top: 30%;
    font-size: 30px;
}
.content{
    width: 100%;
    margin:0 auto;
    height: 460px;
    overflow: hidden;
    position: relative;
    background-color: #fff;
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
    left: 94%;
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
    width: 460px;
    font-size: 22px;
}
#post button{
    position: absolute;
    outline: none;
    border: 0px solid #a5bcf5;
    border-radius: 50%;
    margin: 0;
    padding: 0;
    background-color: rgb(227, 236, 250);
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
    background-color: rgb(193, 213, 247);
}
/* @media screen and (min-width: 1200px){
    .content{
        width: 448px;
    }
    #board{
        width: 580px;
    }
} */
</style>