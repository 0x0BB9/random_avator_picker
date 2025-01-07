<template>
  <div class="min-h-screen tech-bg">
    <!-- 右上角主题切换按钮 -->
    <div class="absolute top-4 right-4">
      <button @click="changeAvatarStyle" 
              class="px-4 py-2 rounded-lg bg-blue-500 hover:bg-blue-600 text-white 
                     transition-all duration-300 backdrop-blur-sm bg-opacity-80">
        切换头像主题 ({{ currentStyle }})
      </button>
    </div>

    <!-- 主要内容区域 -->
    <div class="container mx-auto flex flex-col items-center justify-center min-h-screen p-4">
      <h1 class="text-4xl font-bold mb-12 text-white text-center glow-text">
        幸运抽奖系统
      </h1>

      <!-- 抽奖按钮 -->
      <button @click="startRoll" 
              :disabled="isRolling" 
              class="tech-button mb-16">
        {{ isRolling ? '抽奖中...' : '开始抽奖' }}
      </button>

      <!-- 头像展示区 -->
      <div class="avatar-container">
        <div class="flex gap-4 avatar-scroll"
             :class="{ 'scrolling': isRolling }">
          <div v-for="(user, index) in displayedUsers" 
               :key="index"
               class="avatar-card"
               :class="{ 'winner': winner === index }">
            <img :src="user.avatarUrl" class="w-24 h-24 rounded-lg mb-2">
            <p class="text-center text-white">{{ user.name }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { createAvatar } from '@dicebear/core'
import { lorelei, thumbs, bottts, avataaars } from '@dicebear/collection'

const users = ref([
  { name: '张三', avatarUrl: '' },
  { name: '李四', avatarUrl: '' },
  { name: '王五', avatarUrl: '' },
  { name: '赵六', avatarUrl: '' },
  { name: '钱七', avatarUrl: '' },
  { name: '孙八', avatarUrl: '' },
])

const styles = {
  'lorelei': lorelei,
  'thumbs': thumbs,
  'bottts': bottts,
  'avataaars': avataaars,
}

const currentStyle = ref('lorelei')
const isRolling = ref(false)
const winner = ref(null)

// 生成头像
const generateAvatars = () => {
  users.value.forEach((user, index) => {
    const avatar = createAvatar(styles[currentStyle.value], {
      seed: user.name,
    })
    user.avatarUrl = avatar.toDataUriSync()
  })
}

// 切换头像风格
const changeAvatarStyle = () => {
  const styleNames = Object.keys(styles)
  const currentIndex = styleNames.indexOf(currentStyle.value)
  const nextIndex = (currentIndex + 1) % styleNames.length
  currentStyle.value = styleNames[nextIndex]
  generateAvatars()
}

// 修改 startRoll 函数
const displayedUsers = ref([])

const startRoll = async () => {
  isRolling.value = true
  winner.value = null
  
  // 依次显示所有用户
  displayedUsers.value = []
  for (const user of users.value) {
    displayedUsers.value.push(user)
    await new Promise(resolve => setTimeout(resolve, 300))
  }
  
  // 滚动动画
  for (let i = 0; i < 20; i++) {
    winner.value = Math.floor(Math.random() * users.value.length)
    await new Promise(resolve => setTimeout(resolve, 100 + i * 20))
  }
  
  const finalWinner = Math.floor(Math.random() * users.value.length)
  winner.value = finalWinner
  isRolling.value = false
}

// 初始化时只显示一个默认头像
onMounted(() => {
  generateAvatars()
  displayedUsers.value = [users.value[0]]
})
</script>

<style scoped>
.tech-bg {
  background: linear-gradient(135deg, #1a1a1a 0%, #0a192f 100%);
  position: relative;
  overflow: hidden;
}

.tech-bg::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: 
    linear-gradient(90deg, #00f2fe 0%, #4facfe 100%),
    repeating-linear-gradient(45deg, transparent 0, transparent 10px, rgba(0,0,0,0.1) 10px, rgba(0,0,0,0.1) 20px);
  opacity: 0.1;
  pointer-events: none;
}

.glow-text {
  text-shadow: 0 0 10px rgba(255,255,255,0.5);
}

.tech-button {
  padding: 1rem 3rem;
  font-size: 1.25rem;
  background: linear-gradient(45deg, #4facfe 0%, #00f2fe 100%);
  border: none;
  border-radius: 8px;
  color: white;
  cursor: pointer;
  transition: all 0.3s ease;
  position: relative;
  overflow: hidden;
  box-shadow: 0 0 20px rgba(79,172,254,0.5);
}

.tech-button:hover {
  transform: translateY(-2px);
  box-shadow: 0 0 30px rgba(79,172,254,0.7);
}

.tech-button:disabled {
  opacity: 0.7;
  cursor: not-allowed;
}

.avatar-container {
  width: 100%;
  max-width: 800px;
  overflow: hidden;
  padding: 20px;
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  border-radius: 16px;
  box-shadow: 0 0 20px rgba(0,0,0,0.2);
}

.avatar-card {
  background: rgba(255,255,255,0.1);
  padding: 1rem;
  border-radius: 12px;
  transition: all 0.3s ease;
  backdrop-filter: blur(5px);
}

.winner {
  transform: scale(1.1);
  box-shadow: 0 0 30px rgba(79,172,254,0.8);
  background: rgba(79,172,254,0.2);
}

.avatar-scroll {
  transition: all 0.3s ease;
}

.scrolling {
  animation: scroll 1s linear infinite;
}

@keyframes scroll {
  0% {
    transform: translateX(0);
  }
  100% {
    transform: translateX(-100px);
  }
}
</style> 