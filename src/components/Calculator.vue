<template>
    <div class="p-3" style="max-width: 400px; margin: 50px auto; background: #234;">
      <div class="w-full rounded m-1 p-3 text-end lead font-weight-bold text-white bg-vue-dark ">
        {{ calculatorValue || 0}}
      </div>
  
      <div class="row g-0">
        <div class="col-3" v-for="n in calculatorElements" :key="n">
          <div class="lead text-white text-center bg-vue-dark py-3 m-1 rounded hover-class"
            :class="{'bg-vue-green':['C','*','/','-','+','%','=']}"
            @click="action(n)"
          >
            {{ n }}
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    // eslint-disable-next-line vue/multi-word-component-names
    name: 'Calculator',
    data() {
      return {
        calculatorValue: '',
        calculatorElements: ['C', '*', '/', '-', '7', '8', '9', '+', '4', '5', '6', '%', '1', '2', '3', '=', '0', '.'],
        operator: null,
        previousCalculatorValue: '',
      }
    },
    
    methods: {
      action(n) {
        if (!isNaN(n) || n === '.') {
          this.calculatorValue += n;
        }
  
        if (n === 'C') {
          this.calculatorValue = '';
          this.previousCalculatorValue = '';
          this.operator = null;
        }
  
        if (n === '%') {
          this.calculatorValue = (parseFloat(this.calculatorValue) / 100).toString();
        }
        
        if (['/', '+', '-', '*'].includes(n)) {
          this.operator = n;
          this.previousCalculatorValue = this.calculatorValue;
          this.calculatorValue = this.operator;
        }
  
        if (n ==='=') {
          if (this.operator && this.previousCalculatorValue !== '') {
            if (this.operator === '/' && this.calculatorValue === '0') {
              this.calculatorValue = 'error';
            } else {
              this.calculatorValue = eval(this.previousCalculatorValue + this.calculatorValue).toString();
            }
            this.previousCalculatorValue = '';
            this.operator = null;
          }
        }
      }
    },
  }
  </script>
  
  <style scoped>
  .bg-vue-dark {
    background: #31475e;
  }
  
  .hover-class:hover {
    cursor: pointer;
    background: #3d5875;
  }
  
  .bg-vue-green {
    background: #3fb984;
  }
  </style>
  