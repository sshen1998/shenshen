<template>
	<view class="container">
		<view class="left-bottom-sign"></view>
		<view class="back-btn yticon icon-zuojiantou-up" @click="navBack"></view>
		<view class="right-top-sign"></view>
		<!-- 设置白色背景防止软键盘把下部绝对定位元素顶上来盖住输入框等 -->
		<view class="wrapper">
			<view class="left-top-sign">LOGIN</view>
			<view class="welcome">
				欢迎回来！
			</view>
			<view class="nav_a">
				<view class="nav_b" @click="changclick(index)" :class="{actvie:change==index}" v-for="(item,index) in nav" :key="index">{{item.title}}</view>
			</view>

			<view class="input-content">
				<view class="input-item">
					<text class="tit">{{navtext[change].title}}</text>
					<input type="number" v-model="mobile" :placeholder="navtext[change].name" />
				</view>
				<view class="input-item">
					<text class="tit">{{navtext[change].password}}</text>
					<input type="password" v-model="password" :placeholder="navtext[change].word" />
					<wyb-button class="btn" width="190rpx" type="default" v-if="isshow" @click="yzm()">获取验证码</wyb-button>
				</view>
			</view>
<!-- <wyb-button class="btn" width="190rpx" type="default" v-if="isshow" @click="yzm()">获取验证码</wyb-button> -->
			<!-- <button class="confirm-btna" v-if="isshow" @click="yzm()">获取验证码</button> -->
			<button class="confirm-btn" @click="toLogin()">登录</button>
			<button class="confirm-btn" type="default" @click="tuichu()">退出</button>
			<view class="forget-section" @click="forgets">
				忘记密码?
			</view>
		</view>
		<view class="register-section">
			还没有账号?
			<text @click="toRegist">马上注册</text>
		</view>
	</view>
</template>

<script>
	import {
		mapMutations
	} from 'vuex';

	export default {
		data() {
			return {
				nav: [{
						title: "手机号密码登录",
						index: 0
					},
					{
						title: "手机号验证码登录",
						index: 1
					}
				],
				navtext: [{
						title: "手机号",
						name: "请输入手机号",
						password: "密码",
						word: "8-18位不含特殊字符的数字",
						Index: 0
					},
					{
						title: "手机号码",
						name: "请输入手机号",
						password: "验证码",
						word: "请输入手机验证码",
						Index: 1
					}
				],
				change: 0,
				indexa: 0,
				isshow: false,
				mobile: '',
				password: '',
				logining: false,
				control: "User",
				action: "logIn"
			}
		},
		onLoad() {

		},
		methods: {


			yzm() {
				uni.request({
					url: 'https://yohigame.cnyouwei.com/app.php', //仅为示例，并非真实接口地址。
					method: "POST",
					data: {
						 control:"User",
						      action:"sendSMS",
						mobile: this.mobile

					},
					header: {
						'Content-Type': 'application/x-www-form-urlencoded' //自定义请求头信息
					},
					success: (res) => {

						console.log(res.data);
						console.log(res.data.code);
						if (res.data.code === 1) {
							uni.showToast({
								title: '验证码已发送',
								duration: 2000
							});
						}
					}
				});
				console.log("验证码")
			},
			changclick(index) {
				console.log(123)
				this.change = index
				if (index == 1) {
					this.isshow = true
				} else {
					this.isshow = false
				}
			},
			...mapMutations(['login']),
			inputChange(e) {
				const key = e.currentTarget.dataset.key;
				this[key] = e.detail.value;
			},
			navBack() {
				uni.navigateBack();
			},
			toRegist() {
				this.$api.msg('去注册');
				uni.navigateTo({
					url: "../zhucea/zhuce"
				})
			},
			forgets() {
				uni.navigateTo({
					url: '../forget/forget'
				})
			},
			async toLogin() {

				//密码登录
				if (this.change == 0) {
					console.log(this.mobile)
					console.log(this.password)
					uni.request({
						url: 'https://yohigame.cnyouwei.com/app.php', //仅为示例，并非真实接口地址。
						method: "POST",
						data: {
							mobile: this.mobile,
							password: this.password,
							control: "User",
							action: "logIn",
							control: "User",
							action: "logIn",
							mobile: this.mobile,
							password: this.password
						},
						header: {
							'Content-Type': 'application/x-www-form-urlencoded' //自定义请求头信息
						},
						success: (res) => {
							console.log(res)
							console.log(res.data.result.loginKey);
							console.log(res.data.status)
							uni.setStorageSync('token', res.data.result.loginKey)
							if (res.data.status === 1) {
								uni.switchTab({
									url: '../user/user'
								})
								uni.setStorageSync('names', res.data.result.mobile);
								uni.setStorageSync('status',res.data.status);

							} else {
								uni.showToast({
									title: '用户名或密码错误',
									duration: 2000
								});
							}
						}
					});
				} else {
					//短信登录
					uni.request({
						url: 'http://yishengsuoyi.cnyouwei.com/index/index/smslogin', //仅为示例，并非真实接口地址。
						method: "POST",
						data: {
							mobile: this.mobile,
							sms: this.password
						},

						header: {
							'Content-Type': 'application/x-www-form-urlencoded', //自定义请求头信息
						},
						success: (res) => {

							console.log(res.data.code);
							if (res.data.code === 1) {
								uni.showToast({
									title: '登录成功',
									duration: 2000
								});
								uni.navigateTo({
									url: "../index/index"
								})
							}


						}
					});
					console.log(567)
				}

				this.logining = true;
				const {
					mobile,
					password
				} = this;

			}
		},

	}
</script>

<style lang='scss'>
	.nav_a {
		width: 100%;
		height: 50upx;
		position: relative;
		left: 30px;
		top: -20px;
		display: flex;
	}

	.nav_b {
		width: 40%;
		height: 40upx;
		border: 1px solid #F8F6FC;
		border-radius: 20rpx;
		text-align: center;
	}

	.actvie {
		color: #ec0000;
	}

	page {
		background: #fff;
	}

	.container {
		padding-top: 115px;
		position: relative;
		width: 100vw;
		height: 100vh;
		overflow: hidden;
		background: #fff;
	}

	.wrapper {
		position: relative;
		z-index: 90;
		background: #fff;
		padding-bottom: 40upx;
	}

	.back-btn {
		position: absolute;
		left: 40upx;
		z-index: 9999;
		padding-top: var(--status-bar-height);
		top: 40upx;
		font-size: 40upx;
		color: $font-color-dark;
	}

	.left-top-sign {
		font-size: 120upx;
		color: $page-color-base;
		position: relative;
		left: -16upx;
	}

	.right-top-sign {
		position: absolute;
		top: 80upx;
		right: -30upx;
		z-index: 95;

		&:before,
		&:after {
			display: block;
			content: "";
			width: 400upx;
			height: 80upx;
			background: #b4f3e2;
		}

		&:before {
			transform: rotate(50deg);
			border-radius: 0 50px 0 0;
		}

		&:after {
			position: absolute;
			right: -198upx;
			top: 0;
			transform: rotate(-50deg);
			border-radius: 50px 0 0 0;
			/* background: pink; */
		}
	}

	.left-bottom-sign {
		position: absolute;
		left: -270upx;
		bottom: -320upx;
		border: 100upx solid #d0d1fd;
		border-radius: 50%;
		padding: 180upx;
	}

	.welcome {
		position: relative;
		left: 50upx;
		top: -90upx;
		font-size: 46upx;
		color: #555;
		text-shadow: 1px 0px 1px rgba(0, 0, 0, .3);
	}

	.input-content {
		padding: 0 60upx;
	}

	.input-item {
		position: relative;
		display: flex;
		flex-direction: column;
		align-items: flex-start;
		justify-content: center;
		padding: 0 30upx;
		background: $page-color-light;

		height: 120upx;
		border-radius: 4px;
		margin-bottom: 50upx;

		&:last-child {
			margin-bottom: 0;
		}

		.tit {
			height: 50upx;
			line-height: 56upx;
			font-size: $font-sm+2upx;
			color: $font-color-base;
		}

		input {
			height: 60upx;
			font-size: $font-base + 2upx;
			color: $font-color-dark;
			width: 100%;
		}
		
	}

	.btn {
		position: absolute;
		/* top: 24px; */
		/* left: 480upx; */
		bottom: 10upx;
		right: 10%;
		width: 300upx;
		height: 30upx;
		line-height: 76upx;
		border-radius: 50px;
		background: $uni-color-primary;
		color: #fff;
		font-size: $font-lg;

		&:after {
			border-radius: 100px;
		}
	}

	.confirm-btn {
		width: 630upx;
		height: 76upx;
		line-height: 76upx;
		border-radius: 50px;
		margin-top: 70upx;
		background: $uni-color-primary;
		color: #fff;
		font-size: $font-lg;

		&:after {
			border-radius: 100px;
		}
	}

	.forget-section {
		font-size: $font-sm+2upx;
		color: $font-color-spec;
		text-align: center;
		margin-top: 40upx;
	}

	.register-section {
		position: absolute;
		left: 0;
		bottom: 50upx;
		width: 100%;
		font-size: $font-sm+2upx;
		color: $font-color-base;
		text-align: center;

		text {
			color: $font-color-spec;
			margin-left: 10upx;
		}
	}
</style>
