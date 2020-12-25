<template>
	<view class="content-detaile">

				<button class="btn" open-type="getUserInfo" @getuserinfo="bindGetUserInfo">授权</button>
                <view class="hideCanvas"><canvas canvas-id="myCanvas"></canvas></view>
				 <image :src="posterFile" mode="widthFix" ></image>
	</view>
</template>

<script>
	const {
		windowWidth,
		windowHeight
	} = uni.getSystemInfoSync();
	export default {
		data() {
			return {
				//画图队列
				drawQueue: [{
						path: "https://api.17hl.com.cn/uploads/images/20200528/3df142a3d9d171b6e48e135dff564681.jpg",
						x: 0,
						y: 0,
						width: 800,
						height: 400,
						type: "image",
					},
					{
						text: "禅城文华会物业",
						size: 50,
						x: 30,
						y: 480,
						color: "#000",
						type: "text",
					},
					{
						text: "禅城石湾",
						size: 32,
						x: 30,
						y: 550,
						color: "#666",
						type: "text",
					},

					{
						text: "纯写字楼",
						size: 26,
						x: 30,
						y: 610,
						color: "#9a9a9a",
						type: "text",
					},

					{
						text: "随时看场",
						size: 26,
						x: 160,
						y: 610,
						color: "#9a9a9a",
						type: "text",
					},

					{
						text: "随时入驻",
						size: 26,
						x: 290,
						y: 610,
						color: "#9a9a9a",
						type: "text",
					},

					{
						text: "可注册",
						size: 26,
						x: 420,
						y: 610,
						color: "#9a9a9a",
						type: "text",
					},
					{
						text: "1800元",
						size: 32,
						x: 50,
						y: 660,
						color: "#3197fb",
						type: "text",
					},
					{
						text: "一个月",
						size: 22,
						x: 70,
						y: 700,
						color: "#999",
						type: "text",
					},
					{
						text: "3元",
						size: 32,
						x: 270,
						y: 660,
						color: "#3197fb",
						type: "text",
					},
					{
						text: "1平方/月",
						size: 23,
						x: 250,
						y: 700,
						color: "#999",
						type: "text",
					},
					{
						text: "73平方米",
						size: 32,
						x: 430,
						y: 660,
						color: "#3197fb",
						type: "text",
					},
					{
						text: "面积",
						size: 23,
						x: 480,
						y: 700,
						color: "#999",
						type: "text",
					},
					{
						path: "https://api.17hl.com.cn/uploads/images/20200528/3df142a3d9d171b6e48e135dff564681.jpg",
						x: 480,
						y: 760,
						width: 90,
						height: 90,
						type: "image",
					},
					{
						path: "https://api.17hl.com.cn/uploads/wx_qr/3@o-l9e5YYlc6ujgdOACA48Y9m1d-M.jpg",
						x: 30,
						y: 760,
						width: 90,
						height: 90,
						type: "image",
					},
					{
						text: "好友 张先生",
						size: 23,
						x: 150,
						y: 790,
						color: "#000",
						type: "text",
					},
					{
						text: "推荐您享受内部优惠",
						size: 23,
						x: 150,
						y: 840,
						color: "#999",
						type: "text",
					},
				],
				ctx: null, //画布上下文
				counter: -1, //计数器
				drawPathQueue: [], //画图路径队列
				posterFile: "",
				overView: true,
				showPoster: false,
				showShare: false,
				windowWidth: 0,
				windowHeight: 0,
			
			};
		},
		onReady() {
			const res = uni.getSystemInfoSync();
			this.windowWidth = res.windowWidth;
			this.windowHeight = res.windowHeight;
		},
		onLoad(option) {
			this.ctx = uni.createCanvasContext("myCanvas", this);
		},

		computed: {
			myPx() {
				return windowWidth / 750;
			},
		},
		watch: {
			drawPathQueue(newVal, oldVal) {
				//先画一个白色的画布背景
				this.ctx.fillStyle = "#ffffff";
				this.ctx.fillRect(0, 0, 600 * this.myPx, 880 * this.myPx);
				/* 所有元素入队则开始绘制 */
				if (newVal.length === this.drawQueue.length) {
					console.log("生成的队列：" + JSON.stringify(newVal));
					console.log("开始绘制...");
					for (let i = 0; i < this.drawPathQueue.length; i++) {
						for (let j = 0; j < this.drawPathQueue.length; j++) {
							let current = this.drawPathQueue[j];
							/* 按顺序绘制 */
							if (current.index === i) {
								/* 文本绘制 */
								if (current.type === "text") {
									if (current.text) {
										console.log("绘制文本：" + current.text);
										this.ctx.setFillStyle(current.color);
										this.ctx.setFontSize(
											current.size * this.myPx
										);
										this.ctx.fillText(
											current.text,
											current.x * this.myPx,
											current.y * this.myPx
										);
										if (current.type === "btnText") {
											this.ctx.setFillStyle('#EEEEEE')
											this.ctx.fillRect(current.x * this.myPx, current.y * this.myPx, 150, 75);
										}
									}
									this.counter--;
								}
								/* 图片绘制 */
								if (current.type === "image") {
									if (current.path) {
										console.log("绘制图片：" + current.path);
										this.ctx.drawImage(
											current.path,
											current.x * this.myPx,
											current.y * this.myPx,
											current.width * this.myPx,
											current.height * this.myPx
										);
									}
									this.counter--;
								}
							}
						}
					}

					console.log("final counter", this.counter);
				}
			},
			counter(newVal, oldVal) {
				if (newVal === 0) {
					let that = this;
					that.ctx.setStrokeStyle('#EEEEEE')

					that.ctx.moveTo(0, 370);
					that.ctx.lineTo(400, 370);
					that.ctx.stroke()
					that.ctx.draw();
					/* draw完不能立刻转存，需要等待一段时间 */
					setTimeout(() => {
						uni.canvasToTempFilePath({
							canvasId: "myCanvas",
							success: (res) => {
								console.log("in canvasToTempFilePath");
								// 在H5平台下，tempFilePath 为 base64
								console.log("图片已保存至本地：", res.tempFilePath);
								that.posterFile = res.tempFilePath;
								uni.hideLoading();
								that.toShowPoster(); //显示海报
							},
						});
					}, 2000);
				}
			},
		},
		methods: {
            /**
             * 生成图片
             */
        createPoster() {
            let that = this;
            uni.getSetting({
                success(res) {
                    console.log(res.authSetting, "res.authSetting");
                    if (res.authSetting["scope.userInfo"]) {
                        //如果可以获取
                        wx.getUserInfo({
                            success: (res) => {
                                console.log(res.userInfo, "res.userInfo");
                                if (res.userInfo.avatarUrl) {
                                    that.drawQueue[7].path =
                                        res.userInfo.avatarUrl;
                                } else {
                                    that.drawQueue[7].path = "";
                                }
                                that.drawQueue[8].text = res.userInfo.nickName;

                                // that.share();
								console.log(that.drawQueue, 'that.drawQueue');
                                if (that.posterFile) {
                                    that.toShowPoster(); //显示海报
                                } else {
                                    uni.showLoading({
                                        title: "生成海报中...",
                                    });
                                    that.generateImg();
                                }
                            },
                        });
                    } else {
                        that.showDraw();
                    }
                },
            });
        },
            /***
             *  循环数组生成图片
             */
			generateImg() {
				this.counter = this.drawQueue.length;
				this.drawPathQueue = [];
				/* 将图片路径取出放入绘图队列 */
				for (let i = 0; i < this.drawQueue.length; i++) {
					let current = this.drawQueue[i];
					current.index = i;
					/* 如果是文本直接放入队列 */
					if (current.type === "text") {
						this.drawPathQueue.push(current);
						continue;
					}
					/* 图片需获取本地缓存path放入队列 */
					console.log("将图片路径取出放入绘图队列");
					if (current.type === "image") {
						if (current.path) {
							uni.getImageInfo({
								src: current.path,
								success: (res) => {
									current.path = res.path;
									this.drawPathQueue.push(current);
								},
								fail: (err) => {
									console.log(err, "getImageInfo-err");
								},
							});
						} else {
							
							this.drawPathQueue.push(current);
						}
					}
				}
            },

			//获取用户信息授权
			bindGetUserInfo(e) {
				this.createPoster();
			},
		},
	};
</script>

<style lang="scss" scoped>
	.content-detaile {
		width: 100%;
		background-color: #fff;
		height: 100vh ;


		.hideCanvas {
			width: 100%;
			height: 0;
			overflow: hidden;
			position: absolute;
			left: 800px;
			bottom: 800px;
		}

		canvas {
			width: 600rpx;
			height: 880rpx;
			margin: 0 auto;
		}
	}
</style>
