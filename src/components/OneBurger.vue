<template>
  <div>
      <div class="burger-box">
          <h3>{{ burger.name }}</h3>
          <img v-bind:src="burger.url" :alt="burger.name" style="width:350px; height:285px;">
          <ul>
              <li>{{ burger.kCal }} kCal</li>
              <li v-if="burger.lactose">Contains <span class="allergy">lactose</span></li>
              <li v-if="burger.gluten">Contains <span class="allergy">gluten</span></li>
          </ul>
      </div>
      <div class="amount-row">
          <span class="order-count">Amount:</span>
          <button v-on:click="decrease">−</button> 
          <span class="order-count">{{ amountOrdered }}</span>
          <button v-on:click="increase">+</button>
      </div>
      
  </div>
</template>

<script>
export default {
  name: 'OneBurger',
  props: {
    burger: Object
  },
  data: function () {
  return {
    amountOrdered: 0,
  }
  },
  methods: {
    increase: function () {
      this.amountOrdered += 1;
      this.$emit('orderedBurgers', { name:   this.burger.name, 
                                    amount: this.amountOrdered 
                                  }
                );
    },
    decrease: function () {  
      if (this.amountOrdered > 0) {
        this.amountOrdered -= 1;
        this.$emit('orderedBurgers', { name:   this.burger.name, 
                                      amount: this.amountOrdered 
                                    }
                  );
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .allergy {
      font-weight: bold;
  }

  .order-count {
  font-size: 18px;
  color: rgb(255, 255, 255);
  margin-top: 10px;
  }

  button {
    margin: 5px;
    padding: 8px 14px;
    border-radius: 6px;
    border: 1px solid #444;
    cursor: pointer;
  }

  .amount-row {
    align-items: center;
    vertical-align: bottom;
    display: inline-block;     
    margin-left: 55px;
    padding: 9px 14px;
  }

  .burger-box {
    min-height: 420px; 
    vertical-align: top;
  }
</style>