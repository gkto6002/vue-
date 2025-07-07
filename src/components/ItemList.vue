<script setup lang="ts">
import { ref, computed } from 'vue' // computedをインポート

interface Item {
  name: string
  price: number
}

const items = ref<Item[]>([
  { name: 'たまご', price: 100 },
  { name: 'りんご', price: 160 }
])
const newItemName = ref('')
const newItemPrice = ref(0)

// 追加ボタンを有効にするかどうかを判定する算出プロパティ
const isAddItemDisabled = computed(() => {
  // newItemNameが空、またはnewItemPriceが0以下の場合はtrue（無効）を返す
  return newItemName.value === '' || newItemPrice.value <= 0
})

const addItem = () => {
  // ここで念のため再チェックすることも可能ですが、ボタンが無効になるので通常は不要です。
  if (isAddItemDisabled.value) {
    return; // もしボタンが有効化されていないのにクリックされたら何もしない
  }
  items.value.push({ name: newItemName.value, price: newItemPrice.value })
  newItemName.value = ''
  newItemPrice.value = 0
}
</script>

<template>
  <div>
    <div>ItemList</div>
    <ul>
      <li v-for="item in items" :key="item.name" :class="{ over500: item.price >= 500 }">
        <div>名前: {{ item.name }}</div>
        <div>{{ item.price }} 円</div>
      </li>
    </ul>
    <div>
      <label>
        名前
        <input v-model="newItemName" type="text" />
      </label>
      <label>
        価格
        <input v-model="newItemPrice" type="number" />
      </label>
      <button @click="addItem">add</button>
    </div>
  </div>
</template>

<style>
.over500 {
  color: red;
}
</style>