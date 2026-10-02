<script setup>
import { ref, computed } from 'vue'

const items = ref([
  { id: 1, name: 'Webサイト制作', unitPrice: 300000, checked: false, qty: 1 },
  { id: 2, name: 'ロゴデザイン', unitPrice: 50000, checked: false, qty: 1 },
  { id: 3, name: '写真撮影', unitPrice: 20000, checked: false, qty: 1 },
  { id: 4, name: 'サーバー保守(月額)', unitPrice: 10000, checked: false, qty: 1 },
  { id: 5, name: 'ライティング(記事1本)', unitPrice: 15000, checked: false, qty: 1 },
])

const subtotal = computed(() =>
  items.value
    .filter((item) => item.checked)
    .reduce((sum, item) => sum + item.unitPrice * item.qty, 0)
)

const taxRate = 0.1
const tax = computed(() => Math.floor(subtotal.value * taxRate))
const total = computed(() => subtotal.value + tax.value)

const yen = (n) => n.toLocaleString('ja-JP')
</script>

<template>
  <main class="container">
    <h1>見積もりシミュレータ</h1>
    <p class="lead">必要な項目にチェックを入れ、数量を変更すると合計が自動で更新されます。</p>

    <table class="items">
      <thead>
        <tr>
          <th></th>
          <th>項目</th>
          <th>単価</th>
          <th>数量</th>
          <th>小計</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in items" :key="item.id">
          <td>
            <input type="checkbox" v-model="item.checked" :id="`item-${item.id}`" />
          </td>
          <td>
            <label :for="`item-${item.id}`">{{ item.name }}</label>
          </td>
          <td>¥{{ yen(item.unitPrice) }}</td>
          <td>
            <input
              type="number"
              min="1"
              v-model.number="item.qty"
              :disabled="!item.checked"
            />
          </td>
          <td>¥{{ yen(item.checked ? item.unitPrice * item.qty : 0) }}</td>
        </tr>
      </tbody>
    </table>

    <section class="summary">
      <div class="row">
        <span>小計</span>
        <span>¥{{ yen(subtotal) }}</span>
      </div>
      <div class="row">
        <span>消費税(10%)</span>
        <span>¥{{ yen(tax) }}</span>
      </div>
      <div class="row total">
        <span>合計</span>
        <span>¥{{ yen(total) }}</span>
      </div>
    </section>
  </main>
</template>

<style scoped>
.container {
  max-width: 640px;
  margin: 0 auto;
  padding: 2rem 1rem;
  font-family: system-ui, sans-serif;
  text-align: left;
}

.lead {
  color: #555;
  margin-bottom: 1.5rem;
}

.items {
  width: 100%;
  border-collapse: collapse;
  margin-bottom: 1.5rem;
}

.items th,
.items td {
  padding: 0.5rem;
  border-bottom: 1px solid #ddd;
  text-align: left;
}

.items input[type='number'] {
  width: 4rem;
}

.summary {
  border-top: 2px solid #333;
  padding-top: 1rem;
}

.summary .row {
  display: flex;
  justify-content: space-between;
  padding: 0.25rem 0;
}

.summary .total {
  font-weight: bold;
  font-size: 1.25rem;
  border-top: 1px solid #ccc;
  margin-top: 0.5rem;
  padding-top: 0.5rem;
}
</style>
