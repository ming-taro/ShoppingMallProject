<template>
  <div id="category">
    <div class="category-inner">
      <h3>전체 상품</h3>
      <div style="margin-top:70px">
        <div class="row gx-5">
          <div class="col-sm" v-for="category in category" :key="category">
            <div class="character-card" @click="changeCategory(category.name)">
              <img :src="category.image" class="card-img-top" alt="category.name">
              <div class="card-footer bg-white border-top-0">
                <h5 class="card-title">{{ category.name }}</h5>
              </div>
            </div>
          </div>
        </div>
      </div>

      <select class="form-select" aria-label="quantity selection" @change="sortProductList($event)">
        <option value="신상품순">신상품순</option>
        <option value="인기순">인기순</option>
        <option value="높은 가격순">높은 가격순</option>
        <option value="낮은 가격순">낮은 가격순</option>
      </select>
    </div>
  </div>

  <div id="product-list">
    <div class="product-list-inner">
      <div class="row g-4">
        <div class="col-4" v-for="(product, index) in productList" :key="product" :index="index" >
          <div class="card">
            <img :src="product.image" class="card-img-top" alt="product.name">
            <div class="card-body">
              <h5 class="card-title">{{ product.name }}</h5>
              <p class="card-text"> {{setComma(product.price)}}원</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>


</template>

<script>
export default {
  data() {
    return {
      category:[
        { image: require('@/assets/all-products/kakaoFriends.jpg'), 
          name: "전체",
        },
        { image: "https://t1.kakaocdn.net/friends/prod/character/character_20220531165241_96.png", 
          name: "라이언",
        },
        { image: "https://t1.kakaocdn.net/friends/prod/character/character_20220531165302_96.png", 
          name: "어피치",
        },
        { image: "https://t1.kakaocdn.net/friends/prod/character/character_20220531165550_96.png", 
          name: "춘식이",
        },
      ],
      products:[
        { image: 'http://ec2-13-125-74-101.ap-northeast-2.compute.amazonaws.com:3000/images/Electronic-Clock-Ryan&Choonsik.jpg', 
          name: "라이언과 춘식이의 전자시계", 
          price: "49000",},
        { image: 'http://ec2-13-125-74-101.ap-northeast-2.compute.amazonaws.com:3000/images/Face-Type-Mini-Cushion-Ryan.jpg',
          name: "라이언 리틀 얼굴쿠션", 
          price: "16000"},
        { image: 'http://ec2-13-125-74-101.ap-northeast-2.compute.amazonaws.com:3000/images/Ice-Mug-Apeach.jpg',  
          name: "시원한아이스머그_어피치", 
          price: 10000},
        { image: "https://bucket-wkx1ed.s3.ap-northeast-2.amazonaws.com/목쿠션어피치.jpg", 
          name: "목쿠션_어피치", 
          price: 12000},
        { image: "https://bucket-wkx1ed.s3.ap-northeast-2.amazonaws.com/자석마스크걸이라이언춘식이.jpg", 
          name: "자석마스크걸이_라이언&춘식이", 
          price: 15000},
        { image: "https://bucket-wkx1ed.s3.ap-northeast-2.amazonaws.com/춘식이피규어주차번호판.jpg", 
          name: "춘식이 피규어주차번호판", 
          price: 16000},
        { image: "https://bucket-wkx1ed.s3.ap-northeast-2.amazonaws.com/머니건라이언춘식이.jpg", 
          name: "머니건_라이언&춘식이", 
          price: 19900},
        { image: "https://bucket-wkx1ed.s3.ap-northeast-2.amazonaws.com/핸디형미니선풍기어피치.jpg", 
          name: "핸디형 미니 선풍기_어피치", 
          price: 22000},
        { image: "https://bucket-wkx1ed.s3.ap-northeast-2.amazonaws.com/춘식이보조배터리.jpg", 
          name: "10000mAh춘식이보조배터리", 
          price: 49000},
        { image: "https://bucket-wkx1ed.s3.ap-northeast-2.amazonaws.com/클래식북무드등레드라이언춘식이.jpg", 
          name: "클래식 북 무드등 레드_라이언&춘식이", 
          price: 39000},
        { image: "https://bucket-wkx1ed.s3.ap-northeast-2.amazonaws.com/클래식북무드등그린라이언춘식이.jpg", 
          name: "클래식 북 무드등 그린_라이언&춘식이", 
          price: 39000}
      ],
      productList:[]
    }
  },
  
  methods: {
    setComma(value) {
      return value.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
    },

    getProductList(){
      let productList;

      for(let index=0; index<this.products.length; index++){
        productList.push(this.products[index]);
      }

      return productList;
    },

    changeCategory(character){
      this.productList = [];

      if(character == '전체'){
        for(let index=0; index<this.products.length; index++){
          this.productList.push(this.products[index]);
        }
      }
      else{
        for(let index=0; index<this.products.length; index++){
          if(this.products[index].name.includes(character)){
            this.productList.push(this.products[index]);
          }
        }
      }
    },

    sortProductList(event){
      let sortType = event.target.value;

      if(sortType == "높은 가격순"){
        this.sortByHighPrice();
      }
      else if(sortType == "낮은 가격순"){
        this.sortByLowPrice();
      }
    },

    sortByLowPrice(){
      this.productList.sort(function(a,b){
        return a.price - b.price
      });
    },

    sortByHighPrice(){
      this.productList.sort(function(a,b){
        return b.price - a.price
      });
    },

    comma(price){
      return String(price).replace(/\B(?=(\d{3})+(?!\d))/g, ",");
    }
  },
}
</script> 

<style scoped>
  #category {
  width: 100%;
  height: 600px;
  } 

  .category-inner {
    width: 900px;
    height: 600px;
    margin: 0 auto;
  }
  
  .character-card img{
    border-radius: 100%;
    cursor: pointer;
  }

  .form-select{
    margin-top: 60px;
    float: right;
    width:140px; 
    font-size:16px;
  }

  #product-list{
    width: 100%;
  }

  .product-list-inner{
    width: 900px;
    margin: 0 auto;
  }
</style>