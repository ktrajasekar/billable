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
              <div class="col-sm-4">
                <div class="invoice-details">
                  <div>Invoice No#</div>
                  <div class="focus-editor" contenteditable>2</div>
                  <div>Date</div>
                  <div class="focus-editor" contenteditable>2</div>
                </div>
              </div>
            </div>
          </section>
          <!-- Item Details -->
          <section class="invoice-item-details">
            <div class="row">
              <div class="col-sm-12">
                <div class="view">
                  <table>
                    <thead>
                      <th>
                        <tr>
                          <td>#</td>
                          <td>Items</td>
                          <td>Quantity</td>
                          <td>Amount</td>
                          <td>Total</td>
                          <td>Action</td>
                        </tr>
                      </th>
                    </thead>
                    <tbody v-if="items.length > 0">
                      <tr v-for="(item, index) in items" :key="index">
                        <td>{{ index + 1 }}</td>
                        <td>
                          <input type="text" v-model="item.name" />
                        </td>
                        <td>
                          <input type="number" v-model="item.quantity" />
                        </td>
                        <td>
                          <input type="number" v-model="item.amount" />
                        </td>
                        <td><input type="number" v-model="item.total" /></td>
                        <td>
                          <button type="button" @click="removeItem">
                            Remove
                          </button>
                        </td>
                      </tr>
                    </tbody>
                  </table>
                  <div class="btn">
                    <button type="button" class="newItem" @click="AddItem">
                      Add new row
                    </button>
                  </div>
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
      items: [
        {
          name: "",
          quantity: 0,
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
      },
      deep: true,
    },
  },
  methods: {
    AddItem() {
      this.items.push({
        name: "",
        quantity: 0,
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
