<template>
	<uni-transition ref="ani" custom-class="transition" :mode-class="modeClass" :styles="styles" :show="show">
		<view class="content-box">
			<view class="title">
				{{data.title||"标题加载中..."}}
			</view>
			<view class="xx">
				<view class="author-name">
					<uni-icons color="green" type="person" size="14"></uni-icons>{{data.author_name||"作者"}}
					<uni-icons color="red" type="fire" size="14"></uni-icons>{{data.views||"暂无浏览"}}
				</view>
				<view class="date">
					<uni-icons color="blue" type="calendar" size="14"></uni-icons>{{data.date||"1970-01-01 00:00:00"}}
				</view>
			</view>
			<mp-html lozy-load="true" container-style="overflow: hidden;" selectable="true" :tag-style="tagStyle"
				:content="data.content"></mp-html>
			<!-- <rich-text :nodes="data.content"></rich-text> -->
			<view class="over">—— The End ——</view>

			<view class="Copyright">
				<text>版权声明：若无特殊注明，《</text><text class="Copyright-text">{{ data.title }}</text><text>》内容为《</text><text
					class="Copyright-text">{{ data.author_name||"作者"}}</text><text>》原创，转载请保留文章出处。如有《</text><text
					class="Copyright-text">{{ data.author_name||"作者"}}</text><text>》转载文章时未注明出处或侵犯您的权益或版权，请联系《</text><text
					class="Copyright-text">{{ data.author_name||"作者"}}</text><text>》，我们将及时清理删除并道歉，谢谢！</text>
			</view>
		</view>
		<view class="foot-content"></view>
	</uni-transition>
</template>

<script>
	import {
		myRequest
	} from '@/api.js';
	import set from '@/setting.js';
	export default {
		data() {
			return {
				tagStyle: {
					blockquote: 'max-width:100%;border-radius:5px;background:#eee;padding:5px;',
					table: 'width:100%;border:1px #eee solid;',
					td: 'border:1px #eee solid;text-align:center;',
					th: 'border:1px #eee solid;background-color:#ffc09f;',
				},
				data: {},
				haibao: "",
				url: '',
				arrays:[0],
				content: "<div style='background:#eee;height:25px;width:50%;border-radius:5px;margin-top:10px;'></div>" +
					"<div style='background:#eee;height:20px;width:80%;border-radius:5px;margin-top:10px;'></div>" +
					"<div style='background:#eee;height:20px;width:70%;border-radius:5px;margin-top:10px;'></div>" +
					"<div style='background:#eee;height:20px;width:50%;border-radius:5px;margin-top:10px;'></div>" +
					"<div style='background:#eee;height:20px;width:90%;border-radius:5px;margin-top:10px;'></div>" +
					"<div style='background:#eee;height:20px;width:30%;border-radius:5px;margin-top:10px;'></div>" +
					"<div style='background:#eee;height:25px;width:50%;border-radius:5px;margin-top:10px;'></div>" +
					"<div style='background:#eee;height:250px;width:100%;border-radius:5px;margin:10px auto;'></div>"
			}
		},
		onLoad(option) {
			this.blog(option.id)
			this.id = option.id;
			this.url = option.url;
			this.show = !this.show
			this.modeClass = 'fade'
		},
		onShow() {
		},
		//	小程序分享
		onShareAppMessage(res) {
			if (res.from === 'button') { // 来自页面内分享按钮
				console.log(res.target)
			}
			return {
				title: this.data.title,
				imageUrl: this.data.cover || this.appData.data.shareimg,
				path: 'pages/blog-info/blog-info?id=' + this.data.id + "&url=" + this.url
			}
		},
		methods: {
			// App分享
			weixin(scene) {
				uni.share({
					provider: "weixin",
					scene: scene,
					title: this.data.title,
					type: 0,
					href: this.url,
					imageUrl: this.data.cover || this.appData.data.shareimg,
					summary: "我正在查看文章" + this.data.title + "，赶紧跟我一起来体验！",
					success: function(res) {
						console.log("success:" + JSON.stringify(res));
					},
					fail: function(err) {
						console.log("fail:" + JSON.stringify(err));
					}
				});
			},
			qq() {
				uni.share({
					provider: "qq",
					type: 0,
					title: this.data.title,
					summary: "我正在查看文章" + this.data.title + "，赶紧跟我一起来体验！",
					imageUrl: this.data.cover || this.appData.data.shareimg,
					href: this.url,
					success: function(res) {
						console.log("success:" + JSON.stringify(res));
					},
					fail: function(err) {
						console.log("fail:" + JSON.stringify(err));
					}
				});
			},
			async blog(e) {
				var arrays = this.arrays
				const res = await myRequest({
					url: '/?rest-api=article_detail',
					method: 'GET',
					data: {
						id: e
					}
				})
				this.data = res.data.data.article
				console.log()
			}
		},
	}
</script>

<style>
	.homelist-item {
		padding: 5px;
	}

	.homelist {
		text-align: center;
	}

	.ft-content {
		display: flex;
		justify-content: space-around;
	}

	.content-box {
		background-color: #fff;
		padding: 20px;
		border-radius: 30px 30px 0px 0px;
		margin-top: 10px;
		min-height: 100vh;
	}

	page {
		font-size: 13px;
		background-color: #F17C67;
	}

	.xx {
		width: 100%;
		display: flex;
		justify-content: space-between;
		opacity: 0.6;
		font-size: 14px;
		margin-top: 10px;
	}

	.title {
		font-weight: 800;
		font-size: 18px;
	}

	.Copyright {
		background: #ddd;
		font-size: 12px;
		font-weight: 100;
		padding: 10px;
		border-radius: 5px;
		opacity: 0.5;
	}

	button {
		margin: 0;
		padding: 0;
		border: 0;
		line-height: 1;
		border-radius: 0;
		display: inline-block;
		background-color: #1d243c;
		color: #fff;
	}

	button::after {
		border: none;
	}

	.Copyright-text {
		color: red;
	}

	.over {
		text-align: center;
		color: #000;
		font-size: 12px;
		font-weight: 100;
		margin: 10px 0px;
		opacity: 0.5;
	}

	.tags {
		display: -webkit-box;
		margin: 10px 0px;
		opacity: 0.5;
	}

	.tag {
		background: #ddd;
		font-size: 12px;
		padding: 2px 5px;
		font-weight: 100;
		border-radius: 15px;
		margin: 3px;
	}

	.foot-content {
		height: 40px;
		background-color: #fff;
	}

	.uni-collapse-item {
		background-color: #1d243c;
		border-radius: 30px 30px 0px 0px;
		box-shadow: #555 1px 1px 10px;
		color: #fff;
	}

	.footer {
		position: fixed;
		bottom: 0;
		z-index: 99;
		width: 100%;
	}

	.ft-content {
		padding: 5px;
	}
</style>
