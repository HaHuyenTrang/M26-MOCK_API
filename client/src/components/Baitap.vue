<template>
  <div>
    <h1>Danh sách sản phẩm ra console Bài 2</h1>

    <form @submit.prevent="handleSubmit" action="">
      <h1>thêm người dùng</h1>
      <div>
        <label for="">student_name</label>
        <input v-model="studentInfo.student_name" type="text" />
      </div>
      <div>
        <label for="">address</label>
        <input v-model="studentInfo.address" type="text" />
      </div>
      <div>
        <label for="">email</label>
        <input v-model="studentInfo.email" type="text" />
      </div>
      <div>
        <label for="">phone</label>
        <input v-model="studentInfo.phone" type="number" />
      </div>
      <div>
        <label for="">status</label>
        true<input v-model="studentInfo.status"  type="radio" />
        false<input v-model="studentInfo.status"  type="radio" />
      </div>
      <div>
        <label for="">created</label>
        <input v-model="studentInfo.created" type="text" />
      </div>
      <button>Thêm</button>
      <button @click="handleEdit(student.id)">cập nhật</button>
    </form>

    <table border="1">
      <thead>
        <tr>
          <th>Id</th>
          <th>Name</th>
          <th>Price</th>
          <th>Quantity</th>
          <th>created</th>
          <th>Option</th>
        </tr>
      </thead>
      <tbody v-for="(student, index) in student" :key="student.id">
        <tr>
          <td>{{ index + 1 }}</td>
          <td>{{ student.student_name }}</td>
          <td>{{ student.address }}</td>
          <td>{{ student.phone }}</td>
          <td>{{ student.created }}</td>
          <td>
            <button @click="getstudentById(student.id)">Info</button>
            <button @click="updateStudentById(student.id)">Edit</button>
            <button @click="handleDelete(student.id)">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import axios from "axios";
import { onMounted, reactive, ref } from "vue";
const student = ref([]);
const fetch = async () => {
  try {
    const response = await axios.get("http://localhost:2005/student");
    student.value = response.data;
    console.log(response.data);
  } catch (err) {}
};
onMounted(() => {
  fetch();
});

const getstudentById = async (id) => {
  try {
    const response = await axios.get(`http://localhost:2005/student/${id}`);
    console.log("Data : ", response.data);
  } catch (error) {
    console.log(error, "không tìm thấy bản ghi");
  }
};

const handleDelete = async (id) => {
  const confirm = window.confirm("xóa người dùng này?");
  if (confirm) {
    try {
      const response = await axios.delete(
        `http://localhost:2005/student/${id}`
      );
      student.value = response.data;
      console.log(response.data);
      fetch();
    } catch (err) {
      console.log("false");
    }
  }
};
const studentInfo = reactive({
  student_name: "",
  address: "",
  email: "",
  phone: 0,
  status: false,
  created: "",
});

const handleSubmit = async () => {
  try {
    const response = await axios.post(
      "http://localhost:2005/student",
      studentInfo
    );
    fetch();
    resetForm();
    console.log(response.data);
  } catch {}
};

const updateStudentById = async (id) => {
  try {
    const response = await axios.get(
      `http://localhost:2005/student/${id}`);
    const data = response.data;

    if (data) {
      studentInfo.student_name = data.student_name;
      studentInfo.address = data.address;
      studentInfo.email = data.email;
      studentInfo.phone = data.phone;
      studentInfo.status = data.status;
      studentInfo.created = data.created;
      console.log(studentInfo.status);
     
    }
 
    console.log(response.data);
  } catch (error) {
    console.error("Error updating student:", error);
    // Handle the error, e.g., show a notification to the user
  }
};

const handleEdit = async () => {
  if (!studentInfo.id) {
    console.error("lỗi");
    return;
  }

  try {
    const response = await axios.patch(`http://localhost:2005/student/${studentInfo.id}`, studentInfo);
    console.log("Cập nhật thành công:", response.data);
    fetch(); 
    resetForm(); 
  } catch (error) {
    console.error("Error updating student:", error);
   
  }
};
</script>

<style></style>
