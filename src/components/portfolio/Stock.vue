<template>
  <div class="col-sm-6 col-md-4">
    <div class="panel panel-info">
      <div class="panel-heading">
        <h3 class="panel-title">
          {{ stock.name }}
          <small
            >(Price: {{ stock.price }} | Quantity: {{ stock.quantity }})</small
          >
        </h3>
      </div>
      <div class="panel-body">
        <div class="pull-left">
          <input
            type="number"
            class="form-control"
            :class="{ danger: insufficientQuantity }"
            placeholder="Quantity"
            v-model="quantity"
          />
        </div>
        <div class="pull-right">
          <button
            class="btn btn-danger"
            @click="sellSpecificStock"
            :disabled="
              insufficientQuantity ||
                quantity <= 0 ||
                Number.isInteger(quantity)
            "
          >
            {{ insufficientQuantity ? "Not enough shares" : "Sell" }}
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapActions } from "vuex";
export default {
  props: ["stock"],
  data() {
    return {
      quantity: 0
    };
  },
  methods: {
    ...mapActions(["sellStock"]),
    sellSpecificStock() {
      const order = {
        stockId: this.stock.id,
        stockPrice: this.stock.price,
        quantity: this.quantity
      };
      //from store
      this.sellStock(order);
      this.quantity = 0;
    }
  },
  computed: {
    insufficientQuantity() {
      return this.quantity > this.stock.quantity;
    }
  }
};
</script>

<style scoped>
.danger {
  border: 1px solid red;
}
</style>
