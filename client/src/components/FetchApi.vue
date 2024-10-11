<template>
    <div>
      <h1>List product</h1>
      <form @submit.prevent="handleSubmit" action="">
        <div>
          <label for="">Name</label>
          <input v-model="productInfo.name" type="text" />
        </div>
        <div>
          <label for="">Price</label>
          <input v-model="productInfo.price" type="number" />
        </div>
        <div>
          <label for="">Quantity</label>
          <input v-model="productInfo.quantity" type="number" />
        </div>
        <button>Thêm</button>
      </form>
      <table border="1">
        <thead>
          <tr>
            <th>Id</th>
            <th>Name</th>
            <th>Price</th>
            <th>Quantity</th>
            <th>Option</th>
          </tr>
        </thead>
        <tbody v-for="product in products" :key="product.id">
          <tr>
            <td>{{ product.id }}</td>
            <td>{{ product.name }}</td>
            <td>{{ product.price }}</td>
            <td>{{ product.quantity }}</td>
            <td>
              <button @click="handleEdit(product.id)">sửa</button>
              <button @click="handleDelete(product.id)">xóa</button>
              <button @click="getProductById(product.id)">Get detail</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </template>
  
  <script setup>
  import { onMounted, reactive, ref } from "vue";
  
  const products = ref([]);
  const loadData = async () => {
    // cách 1
    // fetch("http://localhost:2005/product")
    // .then((respons)=> respons.json())
    // .then((data)=>console.log(data))
    // .catch((err)=>console.log(err))
  
    // cách 2
    try {
      const response = await fetch("http://localhost:2005/product");
      const data = await response.json();
      products.value = data;
    } catch (err) {
      return err;
    }
  };
  onMounted(async () => {
    const data = await loadData();
    console.log(data);
    // products.value = data;
  });
  
  const productInfo = reactive({
    name: "",
    price: 0,
    quantity: 0,
  });
  
  const handleSubmit = async () => {
    const response = await fetch("http://localhost:2005/product", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(productInfo),
    });
  
    console.log(response);
    loadData();
  };
  
  const handleDelete = async (id) => {
    const confirm = window.confirm("chắc chắn muốn xóa sản phẩm này?");
    if (confirm) {
      const response = await fetch(`http://localhost:2005/product/${id}`, {
        method: "DELETE",
      });
      console.log(response);
  
      if (response.status === 201) {
        loadData();
      }
    } else {
      console.log("false");
    }
  };
  
  const handleEdit = async (id) => {
    const response = await fetch(`http://localhost:2005/product/${id}`, {
      method: "GET",
    });
    const data = await response.json();
    console.log(data);
    if (data) {
      productInfo.name = data.name;
      productInfo.price = data.price;
      productInfo.quantity = data.quantity;
    }
  };
  const typeForm = ref("add");
  
  const getProductById = async (id) => {
    const response = await fetch(`http://localhost:2005/product/${id}`, {
      method: "GET",
    });
    const data = await response.json();
  
    if (data) {
      console.log(data);
    } else {
      console.log("Không tìm thấy bản ghi");
    }
  };
  </script>
  
  <style></style>
  