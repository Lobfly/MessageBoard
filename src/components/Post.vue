<template>
	<div id="post">
		<textarea ref="message" @keydown.enter="postMessage"></textarea>
		<button @click="postMessage">
			<img src="../assets/发送.svg" width="30" height="30" />
		</button>
	</div>
</template>

<script>
import supabase from '../ultis/supabase'
export default {
	name: 'Post',
	data() {
		return {
			userName: '',
			avatarUrl: '',
			message: '',
		}
	},
	methods: {
    //加个loading动画
		postMessage: async function() {
			this.message = this.$refs.message.value
      this.$refs.message.value = ''
      this.$refs.message.blur()
			await supabase
				.from('message')
				.insert({
					name: this.userName,
					text: this.message,
					avatar: this.avatarUrl,
				})
      this.$bus.$emit('getData')
      
		},
	},
	created() {
		this.$bus.$on('userInfo', (userName, avatarUrl) => {
			this.userName = userName
			this.avatarUrl = avatarUrl
		})
	},
}
</script>

<style scoped>
#post {
	height: 150px;
	background-color: var(--item_color);
	border-radius: 0 0 30px 30px;
	position: relative;
}
#post textarea {
	position: absolute;
	top: 30%;
	left: 50%;
	transform: translate(-50%, -50%);
	resize: none;
	outline: none;
	border: 1px solid #a5bcf5;
	height: 60px;
	width: 92%;
	font-size: 22px;
}
#post button {
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
	transform: translate(-50%, -50%);
	height: 56px;
	width: 56px;
}
#post button img {
	opacity: 0.8;
}
#post button:active {
	background-color: var(--button_active_color);
}
</style>
