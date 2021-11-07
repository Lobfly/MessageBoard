<template>
  <div id="board">
    <Header></Header>
    <Message></Message>
    <Post></Post>
    <div id="login" @click="login">
          登录
      </div>
    <div @click="quit">
      退出
    </div>
  </div>
</template>

<script>
import supabase from "../ultis/supabase"
import Message from "./Message.vue";
import Header from "./Header.vue";
import Post from "./Post.vue";
export default {
  name: "Board",
  data() {
    return {};
  },
  components: { Message, Header, Post },
  methods: {
    login: async function () {
      await supabase.auth.signIn({
        provider: "github",
      });
    },
    quit: async function () {
      await supabase.auth.signOut()
    }
  },
};
</script>

<style scoped>
#board {
  margin-top: 10px;
  width: 370px;
  border-radius: 10px;
  position: relative;
  /* border: 2px solid var(--line_color); */
}

@media screen and (min-width: 1200px) {
  #board {
    width: 500px;
  }
  .avatar {
    left: 88%;
  }
}
</style>