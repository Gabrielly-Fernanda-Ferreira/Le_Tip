<template>
  
  <div class="list-group">

    <label class="bill"> Conta: </label>
    <label class="field"> {{ sign }} {{ bill.toFixed(2) }} </label>

    <label class="word"> Gorjeta: </label>
    <label class="field"> {{ sign }} {{ calculateTip.toFixed(2) }}</label>

    <label class="word"> Total: </label>
    <label class="field"> {{ sign }} {{ total }} </label>

    <label class="word"> Por Pessoa: </label>
    <label class="field"> {{ sign }} {{ forPeople }} </label>

    <label class="word"> Em R$: </label>
    <label class="field" v-if="this.sign==='€'"> R$ {{ currencyConversion ? (currencyConversion?.conversions[0].rate * this.forPeople).toFixed(2) : '0.00' }}</label>
    <label class="field" v-if="this.sign==='$'"> R$ {{ currencyConversion ? (currencyConversion?.conversions[1].rate * this.forPeople).toFixed(2) : '0.00' }}</label>

  </div> 

</template>


<script>
import gql from 'graphql-tag';

export default 
{
  props: 
  {
    bill: Number,
    tip: Number,
    people: Number,
    sign: String
  },
  computed: 
  {
    calculateTip()
    {
      return ((this.tip/100)*this.bill)
    },
    total()
    {
      return (this.bill + this.calculateTip).toFixed(2)
    },
    forPeople()
    {
      return (this.total/this.people).toFixed(2)
    }
  },
  apollo: {
    currencyConversion: {
      query: gql `
      query
      {
        currencyConversion(baseCurrency: "BRL", convertCurrencies: ["USD", "EUR"]){
          conversions {
            rate
          }
        }
      }
      `
    }
  }
}

</script>


<style>

  .bill
  {
    margin-top: 3px;
    font-size: 18px;
    font-weight: bold;
  }
  
</style>