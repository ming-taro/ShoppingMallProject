<template>
  <!-- 장바구니에 담은 상품 정보 -->
  <ItemList :products="products" 
  @getProductNameToBeDeleted="getProductNameToBeDeleted" 
  @setQuantity="setQuantity"
  @getTotalPrice="getTotalPrice"/>
  <!-- 상품 삭제버튼 클릭시 삭제 여부를 한번 더 물음 -->
  <div class="modal fade" id="undeletableGuidanceModal" tabindex="-1" 
  aria-labelledby="modalTitle" aria-hidden="true">
    <div class="modal-dialog modal-dialog-centered">
      <div class="modal-content">
        <!-- 모달창 안내 문구 -->
        <div class="modal-body">
          <h5 style="margin: 50px 0px 50px 0px">선택하신 상품을 삭제하시겠습니까?</h5>
        </div>
        <!-- 모달창 닫기 버튼 -->
        <div class="modal-footer">
          <button type="button" class="btn btn-light" data-bs-dismiss="modal">취소</button>
          <button type="button" class="btn btn-secondary" data-bs-dismiss="modal"
          @click="deleteProduct(productNameToBeDeleted)">확인</button>
        </div>
      </div>
    </div>
  </div>

  <button type="button" class="buy-btn" @click="goToPurchasePage">{{setComma(this.totalPrice)}}원 주문하기</button>

</template>

<script>
import ItemList from '../components/ItemList.vue';

export default {
  data() {
    return {
      products: this.getShoppingCartData(),
      productNameToBeDeleted: "",
      totalPrice: 0
    }
  },
  components: {
    ItemList
  },
  methods: {
    getShoppingCartData(){
      fetch("http://ec2-13-125-74-101.ap-northeast-2.compute.amazonaws.com:3000/baskets/userbasket", {
      method: "POST",
      headers: {
          "Content-Type": "application/json",
      },
      body: JSON.stringify({
          user_id : this.$cookies.get('user_id'),
      }),
      })
      .then((response) => response.json())
      .then((data) => this.products = data);
    },

    setComma(value) {
      return value.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
    },

    getProductNameToBeDeleted(productName){
      this.productNameToBeDeleted = productName;
    },

    getTotalPrice(totalPrice){
      this.totalPrice = totalPrice;
    },

    deleteProduct(productName){
      for(let index=0; index<this.products.length; index++){
        if(this.products[index].name == productName){
          this.products.splice(index, 1);
          break;
        }
      }
    },

    setQuantity(productName, quantity){
      for(let index=0; index<this.products.length; index++){
        if(this.products[index].name == productName){
          this.products[index].quantity = quantity;
        }
      }
    },

    goToPurchasePage(){
      this.$router.push('/PurchasePage')
    }
  },
}
</script>

<style scoped>

  #menu {
    width: 100%;
    height: 50px;
  } 

  .menu-inner {
    width: 900px;
    height: 50px;
    margin: 0 auto;
    border-bottom: 3px solid;
    border-color: #d2d2d2;
    margin-top: 70px;
    font-size: 30px;
  }
  
  .buy-btn {
    width: 400px;
    height: 100px;
    border: 0;
    background-color: #fb2e45;
    color: white;
    font-size: 30px;
    font-weight: bold;
  }
</style>