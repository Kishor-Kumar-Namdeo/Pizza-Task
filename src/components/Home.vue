<template>
  <div class="text-center">
    <h1>Pizza House</h1>
    <hr />
    <div class="container">
      <div class="order-list-wrapper">
        <h2>Order List</h2>
        <table>
          <thead>
            <th>Customer Name</th>
            <th>No. of Items</th>
            <th>Total Amount</th>
            <th>Status</th>
          </thead>
          <tbody>
            <tr v-if="!orders.length">
              <td colspan="4">No Order Received Yet!</td>
            </tr>
            <template v-else>
              <tr v-for="(order, index) in orders" :key="index">
                <td>{{ order.customer_name }}</td>
                <td>{{ order.qty }}</td>
                <td>{{ order.amount }}</td>
                <td>
                  <span
                    :class="'status-chip chip-' + order.status"
                    @click="changeStauts(index, order.status)"
                  >
                    {{ order.status | statusName }}
                  </span>
                </td>
              </tr>
            </template>
          </tbody>
        </table>
      </div>
      <div class="order-form-wrapper">
        <h2>New Order</h2>
        <form @submit.prevent="addOrder">
          <div class="input-field">
            <input
              type="text"
              placeholder="Customer Name"
              v-model="orderForm.customer_name"
              required
            />
          </div>
          <div class="input-field">
            <input
              type="number"
              min="1"
              placeholder="Quantity"
              v-model="orderForm.qty"
              required
            />
          </div>
          <div class="input-field">
            <input
              type="number"
              min="1"
              placeholder="Amount"
              v-model="orderForm.amount"
              required
            />
          </div>
          <button type="submit">ORDER</button>
        </form>
      </div>
    </div>
  </div>
</template>
<script>
const orderStatus = {
  1: "Order Received",
  2: "Preparing",
  3: "Ready to serve"
};
export default {
  data() {
    return {
      orderForm: {
        customer_name: "",
        qty: 1,
        amount: null,
        status: 1
      },
      orders: []
    };
  },
  filters: {
    statusName(index) {
      return orderStatus[index];
    }
  },
  methods: {
    changeStauts(index, status) {
      if (status < 3) {
        this.orders[index].status = ++status;
      }
    },
    addOrder() {
      this.orders.push(this.orderForm);
      // reset new  order form
      this.orderForm = {
        customer_name: "",
        qty: 1,
        amount: null,
        status: 1
      };
    }
  }
};
</script>
<style lang="scss">
* {
  box-sizing: border-box;
}
.text-center {
  text-align: center;
}
.container {
  display: flex;
  flex-wrap: wrap;
  max-width: 1100px;
  padding-top: 50px;
  margin: auto;
  .order-list-wrapper {
    flex: 1;
    margin-right: 16px;
    padding: 16px;
    border: 1px solid #ccc;
    table {
      width: 100%;
      border-bottom: 1px solid #ccc;
      border-spacing: 0;
      td,
      th {
        padding: 10px;
        border-top: 1px solid #ccc;
      }
    }
  }
  .order-form-wrapper {
    width: 40%;
    margin-left: 16px;
    padding: 16px;
    border: 1px solid #ccc;
    .input-field {
      margin-bottom: 16px;
      input {
        padding: 6px 10px;
        font-size: 14px;
        color: #333;
        display: block;
        width: 100%;
      }
    }
    button {
      padding: 6px 10px;
      font-size: 14px;
      background-color: #333;
      color: #fff;
      display: block;
      width: 100%;
    }
  }
  .status-chip {
    padding: 4px 8px;
    border-radius: 3px;
    background-color: #ddd;
    color: #333;
    cursor: pointer;
    min-width: 100px;
    &.chip-1 {
      background-color: #333;
      color: #fff;
    }
    &.chip-2 {
      background-color: green;
      color: #fff;
    }
    &.chip-3 {
      background-color: yellow;
      color: #333;
      cursor: default;
    }
  }
}
</style>
