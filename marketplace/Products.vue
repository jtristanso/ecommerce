<template>
  <div v-if="data !== null">
    <div class="product-holder" v-for="item, index in data" @click="redirect('marketplace/product/' + item.code)">
      <div class="product-image">
        <img :src="config.BACKEND_URL + item.featured[0].url" v-if="item.featured !== null">
        <i class="fa fa-image" v-else></i>
<!--         <div class="product-wishlist bg-primary" v-if="item.wishlist_flag === false && item.checkout_flag === false">
          <label>
            <i class="fa fa-heart"  @click="addToWishlist(item.id)" v-if="item.wishlist_flag === false && item.checkout_flag === false"></i>  
          </label>
          <label @click="redirect('marketplace/product/' + item.code)" style="border-left: solid 1px #ddd;">View</label>
        </div>
        <div class="product-wishlist bg-primary" v-else>
          <label @click="redirect('marketplace/product/' + item.code)" style="width: 100%;">View</label>
        </div> -->
      </div>
      <div class="product-details">
        <div class="product-title">
          <label style="padding-top: 5px;"><b>{{item.title}}</b></label>
          <label>{{item.description}}</label>
        </div>  
        <div class="product-price">
          <label v-if="item.price !== null">
            <label v-if="item.price.length === 1">PHP {{item.price[0].price}}</label>
            <label v-if="item.price.length > 1">PHP {{item.price[item.price.length - 1].price + ' - ' + item.price[0].price}}</label>
          </label>
        </div>
      </div>
    </div>
  </div>
</template>
<style scoped>
  .product-holder{
    width: 24%;
    float: left;
    height: 300px;
    margin-right: 1%;
    border: solid 1px #ddd;
    margin-bottom: 10px;
    color: #555;
    margin-top: 25px;
  }
  .product-holder:hover{
    cursor: pointer;
    border: solid 1px #ffaa81;
    background: #ffaa81;
    color: #fff;
  }

  .product-image{
    width: 100%;
    float: left;
    position: relative;
    height: 250px;
    text-align: center;
  }

  .product-image img{
    height: 250px;
    float: left;
    width: 100%;
  }
  .product-image .fa-image{
    font-size: 150px;
    line-height: 250px;

  }
  .product-details{
    height: 50px;
    width: 100%;
    float: left;
    border-top: solid 1px #ddd;
  }
  .product-title{
    width: 50%;
    float: left;
    height: 50px;
  }
  .product-title label{
    width: 100%;
    float: left;
    font-size: 12px;
    margin: 0px !important;
    padding-left: 10px;
  }
  .product-price{
    width: 50%;
    float: left;
    height: 50px;
    line-height: 50px;
    font-weight: 600;
    text-align: right;
    padding-right: 5px;
  }

  .product-wishlist{
    height: 50px;
    text-align: center;
    line-height: 50px;
    width: 100%;
    position: absolute;
    top: 50%;
    z-index: 10;
    display: none;
  }

  .product-wishlist:hover, .product-wishlist i:hover, .product-wishlist label:hover{
    cursor: pointer;
  }

  .product-wishlist label{
    line-height: 50px;
    float: left;
    width: 50%;
    text-align: center;
  }
  
  .product-wishlist i{
    font-size: 32px;
    line-height: 50px;
  }

  .product-holder:hover .product-wishlist{
    display: block;
  }

</style>
<script>
import ROUTER from '../../../../router'
import AUTH from '../../../../services/auth'
import CONFIG from '../../../../config.js'
import axios from 'axios'
export default {
  mounted(){
  },
  data(){
    return {
      user: AUTH.user,
      config: CONFIG,
      errorMessage: null
    }
  },
  props: ['data'],
  methods: {
    redirect(parameter){
      ROUTER.push(parameter)
    },
    addToWishlist(id){
      let parameter = {
        payload: 'product',
        payload_value: id,
        account_id: this.user.userID
      }
      $('#loading').css({display: 'block'})
      this.APIRequest('wishlists/create', parameter).then(response => {
        $('#loading').css({display: 'none'})
        this.$parent.retrieve()
      })
    }
  }
}
</script>
