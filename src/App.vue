<template>
<div class="app">

  <div class="container">
    <div class="add-product">
      
        <label for="" class="form-label">Название товара:</label>
        <input type="text" class="form-control" id="name" aria-describedby="helpId" v-model.value="inputName">

        <label for="" class="form-label">Цена за единицу товара:</label>
        <input type="number" class="form-control" id="price" aria-describedby="helpId" v-model.value="inputPrice">
        
        <label for="" class="form-label">Количество товара:</label>
        <input type="number" class="form-control" id="quantity" aria-describedby="helpId" v-model.value="inputQuantity">

        <label for="" class="form-label">Дата приема товара:</label>
        <input type="date" class="form-control" id="dates" aria-describedby="helpId" min="01-01,2022" v-model.value="inputDate">


        <br>

        <div class="d-grid gap-2">
          <button type="button" class="btn btn-primary" @click="checkToEmpty">Добавить товар</button>
        </div>
      
    </div>
    

    <div class="total-price">
        <div class="alert alert-primary" role="alert">
          Общая сумма за товаров: <strong>{{totalPrice}}₸</strong>
        </div>
    </div>

    <div class="products-list" v-for="(product, index) in products">
      <div class="product">
        <span>Порядковый номер: {{index + 1}}</span> <br>
        <span>Название: {{product.productName}}</span> <br>
        <span>Цена: {{product.price}}₸ / за штуку</span> <br>
        <span>Количество: {{product.quantity}}шт</span> <br>
        <span>Дата приема товара: {{product.date}}</span> <br>
        <span>Общая цена: {{product.price * product.quantity}}₸</span> <br>
        <div class="remove">
            <button type="button" name="plus" class="btn btn-delete" @click="removeProduct(index)">Удалить</button>
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
      products: [
        {productName: "Губка-спонж Конняку", price: 1400, quantity:13,date:"11-08-2021"},
      ],
      inputName: "",
      inputPrice: "",
      inputQuantity: "",
      inputDate: "",
      totalPrice: 0
    }
  },
  methods: {
    onInput(e, product) {
      product.quantity = Math.max(0, parseInt(e.target.value) || 0);
  },
  addProduct () {
    this.products.unshift({
      productName:this.inputName,
      price:this.inputPrice,
      quantity:this.inputQuantity,
      date:this.inputDate
    }),
    this.inputName = "", this.inputPrice = "", this.inputQuantity = "", this.inputDate = "";
    this.priceResult ();
  },
  checkToEmpty () {
    if (this.inputName == "") {
      document.querySelector("#name").classList.add("empty");
      return
    }
    if (this.inputPrice == "" || this.inputPrice <= 0) {
      document.querySelector("#name").classList.remove("empty");
      document.querySelector("#price").classList.add("empty");
      return
    }
    if (this.inputQuantity == "" || this.inputQuantity <= 0) {
      document.querySelector("#name").classList.remove("empty");
      document.querySelector("#price").classList.remove("empty");
      document.querySelector("#quantity").classList.add("empty");
      return
    }
    if (this.inputDate == "") {
      document.querySelector("#name").classList.remove("empty");
      document.querySelector("#price").classList.remove("empty");
      document.querySelector("#dates").classList.add("empty");
      return
    }
    else 
      document.querySelector("#name").classList.remove("empty");
      document.querySelector("#price").classList.remove("empty");
      document.querySelector("#dates").classList.remove("empty");
      this.addProduct();
  },
  removeProduct (index) {
    this.products.splice(index, 1),
    this.priceResult ();
  },
  priceResult () {
    this.totalPrice = 0;
    for (let i in this.products) {
      let idx = i
      let itemPrice = this.products[idx].price
      let itemQuantity = this.products[idx].quantity
      this.totalPrice += itemPrice * itemQuantity 
    }
   },
  }, 
  mounted () {
    this.priceResult ()
   }
}
</script>
<style scoped>
  input{
    width: 100%;
    padding: 12px 20px;
    margin: 8px 0;
    display: inline-block;  
    border: 1px solid #ccc;
    border-radius: 4px;
    box-sizing: border-box;
}
.btn-primary{
  width: 100%;
  background-color: #4caf50;
  color: white;
  padding: 14px 20px;
  margin: 8px 0;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.btn-primary:hover {
  background-color: #46994a;
}
.btn-delete{
  width: 100%;
  background-color: #c52121;
  color: white;
  padding: 14px 20px;
  margin: 8px 0;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
.btn-delete:hover{
  background-color: #8b1d1d;
}
.product{
        text-align: center;                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         
}
  div {
    border-radius: 5px;
    background-color: #f2f2f2;
    padding: 20px;
}
  .product {
    margin: 20px 0;
    padding: 10px;
    border: 2px solid black;
    border-radius: 10px;
  }
  .empty {
    border-color: #c52121;
  }

  .add-product label {
    padding-left: 10px;
  }
  
  
</style>