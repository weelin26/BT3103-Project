<template>
  <div>
    <headerCom></headerCom>

    <div id="content">
      <p id="picture">
        <img id="shopImage" v-bind:src="shopInfo[0].img_url" />
      </p>
      <p id="shopTitle">{{ shopInfo[0].name }}</p>

      <div>
        <ul id="shopInfo">
          <li style="text-decoration: underline; text-align: left">Objective:</li>
          <li style="text-align: left">{{ shopInfo[0].description }}</li>
          <li style="text-decoration: underline; text-align: left">Production Materials:</li>
          <li style="text-align: left">{{ shopInfo[0].production_materials }}</li>
          <li style="text-decoration: underline; text-align: left">Contact us:</li>
          <li
            style="text-align: left"
          >Contact us at {{ shopInfo[0].contact }} or {{ shopInfo[0].email }}</li>
        </ul>
      </div>

      <span id="productHeader">Products:</span>

      <ul id="productList">
        <li id="pdt" v-for="product in products" :key="product[0]">
          <img id="productImage" v-bind:src="product[1].img_url" />
          <br />
          <span id="productName">
            {{ product[1].name }}
            <likeBtn v-bind:id="product[0]"></likeBtn>
          </span>

          <span id="productPrice">${{ product[1].price }}</span>

          <span id="productPoints">
            <span id="leafIcon">
              <i class="fa fa-leaf"></i>
            </span>
            {{ product[1].points }} points
          </span>
        </li>
      </ul>
    </div>
    <footercomp></footercomp>
  </div>
</template>

<script>
import { database } from "../firebase";
import Header from "./Header.vue";
import LikeButton from "./LikeButton.vue";
import FooterComponent from "./FooterComponent.vue";

export default {
  data() {
    return {
      shopInfo: [],
      products: [],
      likedProducts: []
    };
  },
  components: {
    likeBtn: LikeButton,
    footercomp: FooterComponent,
    headerCom: Header
  },
  methods: {
    fetchItems: function() {
      database
        .collection("companies")
        .get()
        .then(snapshot => {
          snapshot.docs.forEach(doc => {
            this.shopInfo.push(doc.data());
          });
        });

      database
        .collection("products")
        .get()
        .then(snapshot => {
          snapshot.docs.forEach(doc => {
            this.products.push([doc.id, doc.data()]);
          });
        });
    }
  },
  created() {
    this.fetchItems();
  }
};
</script>

<style scoped>
#content {
  position: relative;
  background: #d8e2dc;
  width: 100%;
  height: 1500px;
  left: 0px;
}

#picture {
  width: 55%;
  height: 35%;
  left: 5%;
  top: 3%;
  border: 5px solid #688a75;
  box-sizing: border-box;

  position: absolute;
}

#shopImage {
  position: absolute;
  width: 100%;
  height: 100%;
  left: 5%;
  top: 10%;
}

#shopTitle {
  position: absolute;
  left: 70%;
  top: 2%;

  font-family: EB Garamond;
  font-style: normal;
  font-weight: 500;
  font-size: 48px;

  display: flex;
  align-items: center;
  text-align: center;
  color: #00565e;
}

#shopInfo {
  position: absolute;
  left: 65%;
  top: 8%;

  font-family: EB Garamond;
  font-style: normal;
  font-weight: 500;
  font-size: 22px;
  line-height: 39px;
  display: flex;
  align-items: center;
  text-align: justify;
  color: #26413c;

  display: flex;
  flex-wrap: wrap;
  list-style-type: none;
  padding: 0;
  align-items: center;
}

#productHeader {
  position: absolute;
  left: 4%;
  top: 45%;

  font-family: EB Garamond;
  font-style: normal;
  font-weight: 500;
  font-size: 40px;
  line-height: 52px;
  display: flex;
  align-items: center;
  text-align: center;

  color: #00565e;
}

#pdt {
  max-width: 28%;
  min-width: 25%;
  padding-top: 5%;
  padding-right: 3%;
}

#productName {
  font-family: EB Garamond;
  font-style: normal;
  font-weight: 500;
  font-size: 20px;
  font-weight: bold;
  line-height: 23px;
  display: flex;
  align-items: center;

  color: #00565e;
}

#productImage {
  width: 250px;
  height: 250px;
}

#productPrice {
  position: absolute;
  width: 163px;
  height: 36px;
  margin-top: 0px;

  font-family: EB Garamond;
  font-style: normal;
  font-weight: 500;
  font-size: 25px;
  line-height: 23px;
  display: flex;

  color: #00565e;
}

#productList {
  position: absolute;
  top: 45%;
  left: 10%;
  display: flex;
  flex-wrap: wrap;
  list-style-type: none;
  padding: 0;
  align-items: center;
}

li {
  flex-grow: 1;
  padding: 10px;
}

#productPoints {
  background-color: #8ec693;
  border-radius: 20px;
  line-height: 20px;
  padding: 6px;

  color: #006d77;
  font-family: EB Garamond;
  font-style: normal;
  font-weight: 500;
  font-size: 16px;

  margin-left: 50%;
}

#leafIcon {
  width: 3%;
  height: 3%;
  border: none;
}
</style>
