<template>
  <div class="page-wrapper">
    <Header @search="executeSearch" />
    <div class="page-content">
      <table>
        <thead class="bg-green-400 text-slate-700">
          <tr>
            <th class="text-center">Nome do Produto</th>
            <th class="text-center">Capa</th>
            <th class="text-center">Console</th>
            <th>Menor Preço</th>
            <th>Preço Médio</th>
            <th>Preço Alto</th>
          </tr>
        </thead>
        <tbody class="productsName" v-for="produto in productName.products" :key="produto.id">
          <tr>
            <td class="font-bold nameTitle">
              <button class="text-left" @click.prevent="getDetailsUri(produto.productName, produto.consoleName, produto.id)">></button> {{ produto.productName }}
            </td>
            <td><img v-bind:src="produto.imageUri"></td>
            <td>{{ produto.consoleName }}</td>
            <td class="text-center">{{ produto.price1 }}</td>
            <td class="text-center">{{ produto.price2 }}</td>
            <td class="text-center">{{ produto.price3 }}</td>
          </tr>
         <table class="py-5 text-center">
          <thead class="productDetailsThead" v-if="produto.id === selectedProductId">
            <tr>
              <th colspan="5" class="text-center">Detalhes do produto</th>
            </tr>
            <tr>
              <th>Preço solto</th>
              <th>Preço completo</th>
              <th>Preço classificado</th>
              <th>Preço somente da caixa</th>
              <th>Preço somente do manual</th>
            </tr>
          </thead>
          <tbody class="productDetailsBody" v-if="productDetails && produto.id === selectedProductId">
            <td>{{ productDetails.price1 }}</td>
            <td>{{ productDetails.price2 }}</td>
            <td>{{ productDetails.price3 }}</td>
            <td>{{ productDetails.price5 }}</td>
            <td>{{ productDetails.price7 }}</td>
          </tbody>
         </table>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
  export default{
    name: 'mainComponent',
    data(){
      return{
        productName: {
        products: []
      },
        productDetails: {
          price1: '',
          price2: '',
          price3: '',
          price5: '',
          price7: ''
        },
        selectedProductId: null
    }
    },


    methods: {
      async executeSearch(value) {
        this.productName = await this.$axios.$get(`https://www.pricecharting.com/search-products?q=${value}&type=prices`);
        
        if(this.productName.products.length === 0){
          alert('Não foram encontrados produtos com este nome')
        }

      },

    async getDetailsUri(productName, consoleName, productId){ 
      const uriDetails = (consoleName + '/' + productName).replace(/[\s#()]+/g, ' ').replace(/\s/g, '-').replace(/-$/, '').toLowerCase()
      const link = `https://www.pricecharting.com/pt/game/${uriDetails}`


      const details = await this.$axios.$get(link);

      this.selectedProductId = (this.selectedProductId === productId) ? null : productId;

      this.productDetails.price1 = details.price1
      this.productDetails.price2 = details.price2
      this.productDetails.price3 = details.price3
      this.productDetails.price5 = details.price5
      this.productDetails.price7 = details.price7
    }


  }
  
}
</script>

<style>
*{
  font-family: 'Courier New', Courier, monospace;
}

.page-wrapper {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

table {
  border-collapse: collapse;
}

.price{
  text-align: center;
}

td, th {
  border: 1px solid black;
  font-size: 20px;
}

td img {
  width: 100px;
  height: auto;
}

.page-content {
  flex: 1;  
  color: rgb(0, 0, 0);
}
</style>