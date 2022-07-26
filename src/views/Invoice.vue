<template>
  <div class="generate-invoice">
    <div class="container">
      <div class="row">
        <div class="col-sm-12">
          <div class="invoice-page">
            <h1>INVOICE</h1>
          </div>
          <!-- Company Profile  -->
          <section class="company-profile">
            <div class="row">
              <div class="col-sm-7">
                <div class="focus-editor" contenteditable>
                  <h2>Example Co.</h2>
                </div>
              </div>
              <div class="col-sm-5 text-end">
                <div contenteditable class="focus-editor">
                  www.example.com <br />info@example.com
                </div>
              </div>
            </div>
          </section>
          <!-- Company Profile  End-->
          <section class="address-information">
            <div class="row">
              <div class="col-sm-8">
                <div class="focus-editor" contenteditable>
                  <p>Address</p>
                </div>
              </div>
              <div class="col-sm-4 ">
                <div class="invoice-details float-end">
                  <div>Invoice No</div>
                  <div class="focus-editor" contenteditable>1</div>
                  <div>Date</div>
                  <div class="focus-editor" contenteditable>{{currentDate()}}</div>
                </div>
              </div>
            </div>
          </section>
          <!-- Item Details -->
          <section class="invoice-item-details">
            <div class="row">
              <div class="col-sm-12">
                <div class="view">
                  <table class="table align-middle table-bordered">
                    <thead class="thead-light">
                      <tr class="invoice-border-th">
                        <td scope="col" width="5%">#</td>
                        <td scope="col" colspan="3" width="50%">Description</td>
                        <td scope="col" width="15%">Quantity</td>
                        <td scope="col" width="15%">Price</td>
                          <td scope="col" width="5%" class="d-print-none"></td>
                      </tr>
                    </thead>
                    <tbody >
                      <tr v-for="(item, index) in items" :key="index">
                        <td scope="row" width="5%">{{ index + 1 }}</td>
                        <td colspan="3" scope="row" width="50%">
                          <input type="text" v-model="item.name" class="focus-editor desc-txt" />
                        </td>
                        <td scope="row" width="20%">
                          <input type="text" v-model="item.quantity" class="focus-editor" />
                        </td>
                        <td scope="row" width="15%">
                          <input type="text" v-model="item.amount" class="focus-editor" />
                        </td>
                        <td width="5%" class="d-print-none">
                          <div class="remove align-middle" type="button" v-on:click="removeItem">
                          </div>
                        </td>
                      </tr>
                      <tr class="d-print-none"><td colspan="7"> <span class="add-new-row" v-on:click="addItem"></span></td></tr>
                    </tbody>
                  </table>
                </div>
              </div>
            </div>
          </section>
          <!-- Company Profile  End-->
          <section class="address-information">
            <div class="row">
              <div class="col-sm-8"></div>
              <div class="col-sm-4 ">
                <div class="invoice-details float-end">
                  <div>Subtotal</div>
                  <div class="focus-editor" >{{subTotal.toFixed(2)}}</div>
                  <div> 
                    <ul class="tax-grid-container" >
                      <li class="focus-editor" contenteditable="">VAT</li>
                      <li class="percentage-c"><input type="text" class="tax" v-model="tax" >%</li>
                    </ul>
                     </div>
                  <div class="focus-editor">{{percentage}}</div>
                  <div>Total</div>
                  <div class="focus-editor" >{{Number(subTotal + percentage).toFixed(2) }}</div>
                </div>
              </div>
            </div>
          </section>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      subTotal: 0,
      percentage: 0,
      tax:0,
      items: [
        {
          name: "",
          quantity: 1,
          amount: 0,
          total: 0,
        },
      ],
    };
  },
  watch: {
    items: {
      handler(newValue) {
        newValue.forEach((item) => {
          item.total = item.quantity * item.amount;
        });
        this.subTotal = this.items.reduce((prev, curr) => Number(prev) + Number(curr.total),0);
      },
      deep: true,
    },
    subTotal : {
      handler(val) {
        this.percentage = Number(((this.tax / 100) * val).toFixed(2))
      }
    },
    tax: {
      handler() {
        this.percentage = Number(((this.tax / 100) * this.subTotal).toFixed(2))
      }
    }
  },
  methods: {
    currentDate() {
      const current = new Date();
      const months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      const date = current.getDate()+'-'+(months[current.getMonth()])+'-'+current.getFullYear();
      const dateTime = date 
      return dateTime;
    },
    addItem() {
      this.items.push({
        name: "",
        quantity: 1,
        amount: 0,
        total: 0,
      });
    },
    removeItem() {
      this.items.splice(this.items, 1);
    },
  },
};
</script>
