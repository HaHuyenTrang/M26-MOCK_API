<template>
    <div>
      <h1>Danh sách sản phẩm</h1>
      <button @click="handleAddProduct">Thêm mới sản phẩm</button>
      <table border="1">
        <thead>
          <tr>
            <th>Id</th>
            <th>Name</th>
            <th>Price</th>
            <th>Quantity</th>
            <th>Description</th>
            <th>Option</th>
          </tr>
        </thead>
        <tbody v-for="(product, index) in product" :key="product.id">
          <tr>
            <td>{{ index + 1 }}</td>
            <td>{{ product.name }}</td>
            <td>{{ product.price }}</td>
            <td>{{ product.quantity }}</td>
            <td>{{ product.description }}</td>
            <td>
              <button @click="getProductById(product.id)">Info</button>
              <button>Edit</button>
              <button>Delete</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </template>
  
  <script setup>
  import axios from "axios";
  import { onMounted, ref } from "vue";
  
  const product = ref([]);
  console.log("Product : ", product.value);
  
  const fetchData = async () => {
    try {
      const response = await axios.get(" http://localhost:2005/product");
      product.value = response.data;
    } catch (error) {
      // Xửa lí các lỗi
    }
  
    //   axios
    //     .get(" http://localhost:2005/product")
    //     .then((response) => {
    //       product.value = response.data;
    //     })
    //     .catch((error) => console.log(error));
  };
  
  onMounted(() => {
    fetchData();
  });
  
  // Hàm lấy thông tin chi tiết 1 sản phẩm theo id
  const getProductById = async (id) => {
    try {
      const response = await axios.get(`http://localhost:2005/product/${id}`);
      console.log("Data : ", response.data);
    } catch (error) {
      // Xử lý lỗi
    }
  };
  
  // Hàm thêm mới sản phẩm 
  const handleAddProduct = async () => {
      try {
          const response = await axios.post("http://localhost:2005/product", {
              name:"Mèn mén",
              price:"20000",
              quantity: 20,
              description: "Hơi khô"
          })
          console.log("Response : ", response);
          fetchData();
      } catch (error) {
          
      }
  }
  </script>
  
  <style></style>