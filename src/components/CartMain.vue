<template>
		<main class="cart_box">
		    <div class="cart_tip clearfix" v-if="ok">
		        <span>登录后可同步电脑与手机购物车中的商品</span>
		        <router-link to="mine" class="login">登陆</router-link>
		    </div>
		    <div class="cart_content clearfix" v-for="item in cartDatas">
		        <div class="cart_shop clearfix">
		            <div class="cart_check_box">
		                <div class="check_box">

		                </div>
		            </div>
		            <div class="shop_info clearfix">
		                <img src="../assets/images/buy-logo.png" alt="" class="shop_icon">
		                <span class="shop_name">{{item.shop_name}}</span>
		            </div>
		            <div class="cart_free clearfix">
		                <span class="free_tip">优惠券></span>
		            </div>
		        </div>
		        <div class="cart_item">
		            <div class="cart_item_box">
		                <div class="check_box">

		                </div>
		            </div>
		            <div class="cart_detial_box clearfix">
		                <a href="#" class="cart_product_link">
		                    <img v-lazy="item.product_img_url" alt="">
		                </a>
		                <div class="item_names">
		                    <a href="#">
		                        <span>{{item.product_name}}</span>
		                    </a>
		                </div>
		                <div class="cart_weight">
		                    <i class="my_weigth">重量:0.45kg</i>
		                    <span class="my_color">颜色:AT800/16</span>
		                </div>
		                <div class="cart_product_sell">
		                    <span class="product_money">￥<strong class="real_money">{{item.product_uprice}}</strong>.00</span>
		                    <div class="cart_add clearfix">
		                        <span class="my_add">+</span>
		                        <input type="tel" class="my_count" :value="item.goods_num">
		                        <span class="my_jian">-</span>
		                    </div>
		                </div>
		                <div class="cart_del clearfix">
		                    <div class="del_top">
		                    </div>
		                    <div class="del_bottom">
		                    </div>
		                </div>
		            </div>
		        </div>
		      
		    </div>

		</main>
</template>
<script>
	export default{
		data(){
			return{
				cartDatas:[],
				ok: true,
			}
		},
		mounted(){
			this.getCartDatas(),
			this.getUDatas()
		},
		methods:{
			getCartDatas(){
				let _this = this;
				_this.$http.get('/cart').then((res)=>{
					_this.cartDatas = res.data;
				},(err)=>{
					console.log(err);
				})
			},
			getUDatas(){
				let _this = this;
				let uObj ={};
				if(window.sessionStorage.userInfo){
					let uObj = JSON.parse(window.sessionStorage.userInfo);
					let useId = uObj.user_id;
					_this.$http.get('/userinfo',{
						params:{
							uId:useId
						}
					}).then((res)=>{
						_this.ok = false;
						console.log(_this.uInfs);
					},(err)=>{
						console.log(err);
					});
				}else{
					_this.$router.push({
						path:'/login',
					})
				}
			}
		}
	}
</script>
