<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <div class="button_products">
    <button @click="AddCart">add to cart Product</button>
    <button @click="onClick">Next Product</button>
  </div>
</template>

<script>
export default {
  methods: {
    onClick() {
      // ini adalah komponent anak
      this.$emit("next-product");
    },
    AddCart() {
      this.$emit("next-addTocart");
    },
  },
};
</script>

<style>
:root {
  --white: #fff;
  --anti: antiquewhite;
  --purple: #720060;
  --blue: #002772;
}

.button_products {
  margin-top: 0.9rem;
  display: flex;
  gap: 20px;
}
button {
  width: 259px;
  color: aliceblue;
  /* background-color: var(--purple); */
  padding: 0.7rem;
}
button:nth-child(2) {
  width: 259px;
}
button:hover {
  background-color: var(--anti);
  color: black;
}
p {
  font-size: 11px;
}
</style>
