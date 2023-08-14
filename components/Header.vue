<template>
  <div class="bg-green-500 flex justify-center">
    <form class="py-5" >
      <label >
        <span class="text-md font-medium text-slate-700">Informe o jogo que deseja buscar: </span>
        <input class="mx-3 rounded-xl p-2" v-model='inputValue'>
        <button class="bg-green-900 rounded-md p-3.5" @click.prevent="searchBtn"><img src="./assets/searchLogo.svg"></button>
      </label>
    </form>
    </div>
</template>

<script>

export default {
  name: 'headerComponent',
  data(){
    return{
      inputValue: ''
    }
  },

  methods: {
    searchBtn(){
      this.$emit('search', this.inputValue)
    }
  }
}
</script>
