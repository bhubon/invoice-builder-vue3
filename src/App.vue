<script setup>
import { reactive } from 'vue';
import { invoice1, invoice2 } from './data/data.js';

const data = reactive({
  sender: '',
  billTo: '',
  shipTo: '',
  invoiceNumber: '',
  date: '',
  dueDate: '',
  additionalNote: '',
  items: [
    {
      description: '',
      quantity: '',
      rate: '',
      discount: '',
      amount: ''
    }
  ],
  notes: '',
  terms: '',
  subtotal: '',
  tax: '',
  total: '',
})

function addMoreItem() {
  data.items.push({
    removeAble: true,
    description: '',
    quantity: '',
    rate: '',
    discount: '',
    amount: ''
  })
}

function removeItem(index) {
  data.items.splice(index, 1)
}

function getSubtotal() {
  let subtotal = 0;
  data.items.forEach((item) => {
    subtotal += item.amount
  })
  data.subtotal = subtotal
  return subtotal
}

function getTotal() {
  const tax = data.subtotal * data.tax / 100
  const total = data.subtotal + tax
  data.total = total
  return total
}
function saveData() {
  console.log(data)
}

function itemCalculatedAmount(item) {
  let amount = 0;
  amount = (item.quantity * item.rate) - ((item.quantity * item.rate) * item.discount / 100);
  item.amount = amount;
  return amount;
}

</script>

<template>
  <section class="mx-auto container bg-white border border-gray-400 min-h-screen p-12">
    <div class="flex justify-between">
      <div class="flex flex-col space-y-5 w-1/2s">
        <div class=" ">
          <img class="w-40"
            src="https://www.shutterstock.com/image-vector/invoice-typographic-stamp-sign-badge-600w-1027820257.jpg"
            alt="">
        </div>
        <p class="mt-5">
          Sender
        </p>
        <textarea name="" id="" cols="30" rows="2" v-model="data.sender"></textarea>
        <div class="flex space-x-2">
          <div class="flex flex-col">
            <span>Bill to</span>
            <textarea name="" id="" cols="30" rows="2" v-model="data.billTo"></textarea>
          </div>
          <div class="flex flex-col">
            <span>Ship to</span>
            <textarea name="" id="" cols="30" rows="2" v-model="data.shipTo"></textarea>
          </div>
        </div>
      </div>
      <div class="flex flex-col w-1/2 items-end">
        <h1 class="mt-12 text-4xl uppercase text-right mb-5">Invoice</h1>
        <input class="w-[200px] text-right" type="text" placeholder="Invoice Number" v-model="data.invoiceNumber">
        <div class="mt-10 flex-y-5 text-right space-y-3 w-full">
          <p>
            <span>Date</span>
            <input class="ml-2 w-[200px] " v-model="data.date">
          </p>
          <p>
            <span>Due Date</span>
            <input class="ml-2 w-[200px]" v-model="data.dueDate">
          </p>
          <p>
            <span>Additional Note</span>
            <input class="ml-2 w-[200px]" type="text" v-model="data.additionalNote">
          </p>
        </div>
      </div>
    </div>
    <div class="mt-20">
      <table class="table-auto w-full">
        <tr class="bg-gray-800 text-left text-white">
          <th class=""></th>
          <th class="p-2 pl-5 w-1/2">Item</th>
          <th class="p-2">Quantity</th>
          <th class="p-2">Rate</th>
          <th class="p-2">Discount (%)</th>
          <th class="p-2 w-[190px] text-right pr-5">Amount</th>
        </tr>
        <tr v-for="(item, index) in data.items" :key="index">
          <td>
            <button @click="removeItem(index)" :disabled="data.items.length > 1 ? false : 'disabled'"
              :class="data.items.length > 1 ? 'bg-red-700' : 'bg-gray-400'" class="text-white p-1 rounded "><svg
                xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                stroke="currentColor" class="w-6 h-6">
                <path stroke-linecap="round" stroke-linejoin="round" d="M15 12H9m12 0a9 9 0 11-18 0 9 9 0 0118 0z" />
              </svg>
            </button>
          </td>
          <td class="py-1">
            <input class="w-full pl-5" type="text" placeholder="Description" v-model="item.description" />
          </td>
          <td class="">
            <input class="w-full" type="number" placeholder="Quantity" v-model="item.quantity" />
          </td>
          <td class="">
            <input class="w-full" type="number" placeholder="Rate" v-model="item.rate">
          </td>
          <td class="">
            <input class="w-full" type="number" placeholder="Dicount (%)" v-model="item.discount">
          </td>
          <td class="py-1 pr-5 text-right text-gray-800">
            $ {{ itemCalculatedAmount(item) }}
          </td>
        </tr>
      </table>
      <button @click="addMoreItem()" class="mt-5 bg-green-600 hover:bg-green-700 text-white font-bold py-2 px-4 rounded">
        Add More
      </button>
      <button @click="saveData()"
        class="ml-2 mt-5 bg-green-600 hover:bg-green-700 text-white font-bold py-2 px-4 rounded">
        Save
      </button>
      <button @click="Object.assign(data, invoice1)"
        class="ml-2 mt-5 bg-green-600 hover:bg-green-700 text-white font-bold py-2 px-4 rounded">
        Load Invoice 1
      </button>
      <button @click="Object.assign(data, invoice2)"
        class="ml-2 mt-5 bg-green-600 hover:bg-green-700 text-white font-bold py-2 px-4 rounded">
        Load Invoice 2
      </button>
      <!-- <br>
      {{ data }} -->
    </div>
    <div class="mt-[200px]">
      <div class="flex justify-between">
        <div class="flex flex-col space-y-5 w-1/2">
          <span>Notes</span>
          <textarea name="" id="" cols="30" rows="2" v-model="data.notes"></textarea>
          <span>Terms</span>
          <textarea name="" id="" cols="30" rows="2" v-model="data.terms"></textarea>
        </div>
        <div class="flex flex-col w-1/2 items-end">
          <div class="mt-10 flex-y-5 text-right space-y-3 w-full">
            <p>
              <span>Subtotal</span>
              <input readonly class="focus:ring-0 focus:ring-offset-0 text-right ml-2 pr-4 w-[200px] border-0"
                placeholder="Subtotal" :value="getSubtotal()">
            </p>
            <p>
              <span>Tax</span>
              <input type="number" class="tax text-right w-[200px] ml-2" v-model="data.tax">
            </p>
            <p>
              <span>Total</span>
              <input readonly class="focus:ring-0 focus:ring-offset-0 text-right ml-2 pr-4 w-[200px] border-0"
                placeholder="Total" :value="getTotal()">
            </p>
            <!-- <p>
              <span>Balace Due</span>
              <input readonly class="focus:ring-0 focus:ring-offset-0 text-right ml-2 pr-4 w-[200px] border-0"
                placeholder="Balance">
            </p> -->
          </div>
        </div>
      </div>
    </div>

  </section>
</template>

<style scoped>
input,
textarea {
  border: 1px solid #ddd !important;
  padding: 5px;
  border-radius: 5px;
}

input.tax::-webkit-outer-spin-button,
input.tax::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
</style>
