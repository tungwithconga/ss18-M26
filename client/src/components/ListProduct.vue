<template>
    <div>
      <h1>Product List</h1>
      <button @click="getProductById(2)">Get detail</button>
      <button @click="removeProductById(2)">Delete</button>
      <button @click="createProduct()">Create Product</button>
      <button @click="updateProductById(2)">Update Product</button>
      <p v-if="message">{{ message }}</p>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  
  const message = ref('');
  
  // Hàm getProductById để lấy thông tin sản phẩm theo ID
  const getProductById = async (id) => {
    try {
      const response = await fetch(`http://localhost:3000/products/${id}`);
      if (response.ok) {
        const product = await response.json();
        console.log('Thông tin chi tiết sản phẩm:', product);
        message.value = `Sản phẩm: ${product.name}, Giá: ${product.price}`;
      } else {
        console.log('Không tìm thấy bản ghi');
        message.value = 'Không tìm thấy sản phẩm.';
      }
    } catch (error) {
      console.error('Error fetching product:', error);
      message.value = 'Đã xảy ra lỗi khi gọi API.';
    }
  };
  
  // Hàm removeProductById để xóa sản phẩm theo ID
  const removeProductById = async (id) => {
    try {
      const response = await fetch(`http://localhost:3000/products/${id}`, {
        method: 'DELETE'
      });
  
      if (response.ok) {
        console.log(`Sản phẩm với id ${id} đã được xóa.`);
        message.value = `Sản phẩm với id ${id} đã được xóa.`;
      } else {
        console.log(`Không thể xóa sản phẩm với id ${id}.`);
        message.value = `Không thể xóa sản phẩm với id ${id}.`;
      }
    } catch (error) {
      console.error('Error deleting product:', error);
      message.value = 'Đã xảy ra lỗi khi xóa sản phẩm.';
    }
  };
  
  // Hàm createProduct để thêm sản phẩm mới
  const createProduct = async () => {
    const product = {
    id:"6",
      name: "Product New",
      image: "https://example.com/image-new.jpg",
      price: 300,
      quantity: 15
    };
  
    try {
      const response = await fetch('http://localhost:3000/products', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(product), 
      });
  
      if (response.ok) {
        const newProduct = await response.json();
        console.log('Sản phẩm mới đã được thêm:', newProduct);
        message.value = `Sản phẩm mới đã được thêm: ${newProduct.name}`;
      } else {
        console.log('Không thể thêm sản phẩm.');
        message.value = 'Không thể thêm sản phẩm.';
      }
    } catch (error) {
      console.error('Error creating product:', error);
      message.value = 'Đã xảy ra lỗi khi thêm sản phẩm.';
    }
  };
  
  // Hàm updateProductById để cập nhật thông tin sản phẩm theo ID
  const updateProductById = async (id) => {
    const updatedProduct = {
      name: "Updated Product Name",
      image: "https://example.com/image-updated.jpg",
      price: 500,
      quantity: 20
    };
  
    try {
      const response = await fetch(`http://localhost:3000/products/${id}`, {
        method: 'PUT',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(updatedProduct),
      });
  
      if (response.ok) {
        const updatedProductFromServer = await response.json();
        console.log('Sản phẩm đã được cập nhật:', updatedProductFromServer);
        message.value = `Sản phẩm với id ${id} đã được cập nhật: ${updatedProductFromServer.name}`;
      } else {
        console.log(`Không thể cập nhật sản phẩm với id ${id}.`);
        message.value = `Không thể cập nhật sản phẩm với id ${id}.`;
      }
    } catch (error) {
      console.error('Error updating product:', error);
      message.value = 'Đã xảy ra lỗi khi cập nhật sản phẩm.';
    }
  };
  </script>
  
  <style scoped>
  </style>
  