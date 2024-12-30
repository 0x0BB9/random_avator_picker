<template>
    <div class="avatar-list">
      <div v-for="(avatar, index) in avatars" :key="index" class="avatar-item">
        <img :src="avatar" alt="Avatar" />
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        avatars: [
          'https://randomuser.me/api/portraits/men/1.jpg',
          'https://randomuser.me/api/portraits/women/1.jpg',
          'https://randomuser.me/api/portraits/men/2.jpg',
          'https://randomuser.me/api/portraits/women/2.jpg',
          // 添加更多头像链接
        ]
      }
    }
  }
  </script>
  
  <style scoped>
  .avatar-list {
    display: flex;
    flex-wrap: wrap;
  }
  .avatar-item {
    margin: 10px;
  }
  .avatar-item img {
    width: 100px;
    height: 100px;
    border-radius: 50%;
  }
  </style>