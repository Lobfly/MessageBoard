<template>
	<div>
		<div id="header">
			<div class="title">Message Board</div>
			<div class="avatar" ref="avatar"></div>
			<div class="user">
				Welcome
				<span
					>@{{ userName }}
					<img src="../assets/edit.svg" />
				</span>
			</div>
		</div>
	</div>
</template>

<script>
import supabase from '../ultis/supabase'
export default {
	name: 'Header',
	data() {
		return {
			avatarUrl: '',
		}
	},
	computed: {
		userName() {
			return 'user' + Math.floor(Math.random() * 100 + 1)
		},
	},
	methods: {
		randomAvatar: async function() {
			let randomNum = Math.floor(Math.random() * 9 + 1) //生成1到9的随机数
			this.avatarUrl = await supabase.storage
				.from('avatars')
				.getPublicUrl(`${randomNum}.jpg`).publicURL
			this.$refs.avatar.style.backgroundImage = `url(${this.avatarUrl})`
		},
	},
	mounted() {
		this.randomAvatar().then(() => {
			this.$bus.$emit('userInfo', this.userName, this.avatarUrl)
		})
	},
	updated() {
		this.$bus.$emit('userInfo', this.userName, this.avatarUrl)
	},
}
</script>

<style scoped>
#header {
	height: 100px;
	width: 100%;
	position: relative;
	background-color: var(--item_color);
	border-radius: 30px 30px 0 0;
	border-bottom: 1px solid var(--line_color);
	user-select: none;
}
.avatar {
	width: 50px;
	height: 50px;
	background-color: grey;
	border-radius: 50%;
	position: absolute;
	left: 80%;
	top: 22px;
	cursor: pointer;
	/* background-image: url("../assets/Avater/1.jpg"); */
	background-size: cover;
	animation: loading ease-in-out 1.6s infinite;
}
.title {
	position: absolute;
	left: 30px;
	top: 30%;
	font-size: 30px;
}
.user {
	position: absolute;
	left: 30px;
	top: 72px;
}
.user span {
	color: #79b616;
	cursor: pointer;
}
.user img {
	width: 20px;
	height: 20px;
}
</style>
