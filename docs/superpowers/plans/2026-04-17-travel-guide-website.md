# 旅游攻略网站实现计划

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 构建一个旅游攻略网站，包含目的地选择、线路生成、景点/美食展示等功能

**Architecture:** Vue 3 + Vite 单页应用，使用 Pinia 管理状态，Vue Router 处理路由，Tailwind CSS 实现样式

**Tech Stack:** Vue 3, Vite, Vue Router 4, Pinia, Tailwind CSS

---

## 文件结构映射

```
my-site/
├── package.json
├── vite.config.js
├── tailwind.config.js
├── index.html
├── src/
│   ├── main.js
│   ├── App.vue
│   ├── router/index.js
│   ├── stores/itinerary.js
│   ├── data/destinations.js
│   ├── data/attractions.js
│   ├── data/restaurants.js
│   ├── views/Home.vue
│   ├── views/Itinerary.vue
│   ├── views/AttractionDetail.vue
│   ├── views/RestaurantDetail.vue
│   ├── components/DestinationSelector.vue
│   ├── components/ItineraryTimeline.vue
│   ├── components/PlaceCard.vue
│   ├── components/FilterPanel.vue
│   └── utils/itineraryGenerator.js
└── public/
```

---

### Task 1: 初始化 Vue 3 + Vite 项目

**Files:**
- Create: `package.json`
- Create: `vite.config.js`
- Create: `index.html`
- Create: `src/main.js`
- Create: `src/App.vue`

- [ ] **Step 1: 创建 package.json**

```json
{
  "name": "travel-guide-website",
  "version": "1.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  },
  "dependencies": {
    "vue": "^3.4.0",
    "vue-router": "^4.2.0",
    "pinia": "^2.1.0"
  },
  "devDependencies": {
    "@vitejs/plugin-vue": "^5.0.0",
    "autoprefixer": "^10.4.0",
    "postcss": "^8.4.0",
    "tailwindcss": "^3.4.0",
    "vite": "^5.0.0"
  }
}
```

- [ ] **Step 2: 创建 vite.config.js**

```javascript
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

export default defineConfig({
  plugins: [vue()],
  server: {
    port: 3000
  }
})
```

- [ ] **Step 3: 创建 index.html**

```html
<!DOCTYPE html>
<html lang="zh-CN">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>旅游攻略 - 智能线路生成</title>
  </head>
  <body>
    <div id="app"></div>
    <script type="module" src="/src/main.js"></script>
  </body>
</html>
```

- [ ] **Step 4: 创建 src/main.js**

```javascript
import { createApp } from 'vue'
import { createPinia } from 'pinia'
import App from './App.vue'
import router from './router'
import './style.css'

const app = createApp(App)
app.use(createPinia())
app.use(router)
app.mount('#app')
```

- [ ] **Step 5: 创建 src/App.vue**

```vue
<template>
  <div id="app" class="min-h-screen bg-gray-50">
    <nav class="bg-white shadow-sm">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="flex justify-between h-16">
          <div class="flex items-center">
            <router-link to="/" class="text-xl font-bold text-blue-600">
              🌍 旅游攻略
            </router-link>
          </div>
        </div>
      </div>
    </nav>
    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
      <router-view />
    </main>
  </div>
</template>

<script setup>
</script>

<style>
</style>
```

- [ ] **Step 6: 创建 src/style.css**

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  body {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  }
}
```

---

### Task 2: 配置 Tailwind CSS 和路由

**Files:**
- Create: `tailwind.config.js`
- Create: `postcss.config.js`
- Create: `src/router/index.js`

- [ ] **Step 1: 创建 tailwind.config.js**

```javascript
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./index.html",
    "./src/**/*.{vue,js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {
      colors: {
        primary: '#3B82F6',
        secondary: '#F97316',
      }
    },
  },
  plugins: [],
}
```

- [ ] **Step 2: 创建 postcss.config.js**

```javascript
export default {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
}
```

- [ ] **Step 3: 创建 src/router/index.js**

```javascript
import { createRouter, createWebHistory } from 'vue-router'
import Home from '../views/Home.vue'
import Itinerary from '../views/Itinerary.vue'
import AttractionDetail from '../views/AttractionDetail.vue'
import RestaurantDetail from '../views/RestaurantDetail.vue'

const routes = [
  { path: '/', name: 'Home', component: Home },
  { path: '/itinerary', name: 'Itinerary', component: Itinerary },
  { path: '/attraction/:id', name: 'AttractionDetail', component: AttractionDetail },
  { path: '/restaurant/:id', name: 'RestaurantDetail', component: RestaurantDetail },
]

const router = createRouter({
  history: createWebHistory(),
  routes,
})

export default router
```

---

### Task 3: 创建静态数据

**Files:**
- Create: `src/data/destinations.js`
- Create: `src/data/attractions.js`
- Create: `src/data/restaurants.js`

- [ ] **Step 1: 创建 src/data/destinations.js**

```javascript
export const destinations = [
  {
    id: 'tokyo',
    name: '东京',
    country: '日本',
    description: '传统与现代完美融合的大都市',
    image: 'https://images.unsplash.com/photo-1540959733332-eab4deabeeaf?w=800',
    bestSeason: '春季、秋季',
  },
  {
    id: 'kyoto',
    name: '京都',
    country: '日本',
    description: '千年古都，寺庙与禅意的世界',
    image: 'https://images.unsplash.com/photo-1493976040374-85c8e12f0c0e?w=800',
    bestSeason: '春季、秋季',
  },
  {
    id: 'osaka',
    name: '大阪',
    country: '日本',
    description: '美食天堂，充满活力的商业城市',
    image: 'https://images.unsplash.com/photo-1590559899731-a382839e5549?w=800',
    bestSeason: '春季、秋季',
  },
]
```

- [ ] **Step 2: 创建 src/data/attractions.js**

```javascript
export const attractions = {
  tokyo: [
    {
      id: 'tokyo-1',
      name: '浅草寺',
      description: '东京最古老的寺庙，充满传统风情',
      images: ['https://images.unsplash.com/photo-1545569341-9eb8b30979d9?w=600'],
      address: '东京都台东区浅草2-3-1',
      openingHours: '06:00-17:00',
      duration: 90,
      price: 0,
      rating: 4.5,
      tags: ['文化', '寺庙', '历史'],
      bestTimeToVisit: '上午',
      location: { lat: 35.7148, lng: 139.7967 },
    },
    {
      id: 'tokyo-2',
      name: '东京塔',
      description: '东京地标性建筑，可俯瞰城市全景',
      images: ['https://images.unsplash.com/photo-1540959733332-eab4deabeeaf?w=600'],
      address: '东京都港区芝公园4-2-8',
      openingHours: '09:00-23:00',
      duration: 60,
      price: 1200,
      rating: 4.3,
      tags: ['观景台', '地标', '夜景'],
      bestTimeToVisit: '傍晚',
      location: { lat: 35.6586, lng: 139.7454 },
    },
    {
      id: 'tokyo-3',
      name: '秋叶原',
      description: '动漫与电子产品的圣地',
      images: ['https://images.unsplash.com/photo-1542051841857-5f90071e7989?w=600'],
      address: '东京都千代田区外神田',
      openingHours: '10:00-20:00',
      duration: 120,
      price: 0,
      rating: 4.2,
      tags: ['购物', '动漫', '电子产品'],
      bestTimeToVisit: '下午',
      location: { lat: 35.7022, lng: 139.7746 },
    },
    {
      id: 'tokyo-4',
      name: '明治神宫',
      description: '位于繁华都市中的宁静神社',
      images: ['https://images.unsplash.com/photo-1528360983277-13d401cdc186?w=600'],
      address: '东京都涩谷区代代木神园町1-1',
      openingHours: '05:00-18:00',
      duration: 60,
      price: 0,
      rating: 4.6,
      tags: ['神社', '自然', '宁静'],
      bestTimeToVisit: '上午',
      location: { lat: 35.6759, lng: 139.7046 },
    },
    {
      id: 'tokyo-5',
      name: '涩谷十字路口',
      description: '世界最繁忙的十字路口',
      images: ['https://images.unsplash.com/photo-1542051841857-5f90071e7989?w=600'],
      address: '东京都涩谷区道玄坂',
      openingHours: '全天',
      duration: 30,
      price: 0,
      rating: 4.0,
      tags: ['地标', '城市风光', '夜景'],
      bestTimeToVisit: '傍晚',
      location: { lat: 35.6595, lng: 139.7004 },
    },
  ],
  kyoto: [
    {
      id: 'kyoto-1',
      name: '金阁寺',
      description: '金碧辉煌的禅寺，倒映在湖面',
      images: ['https://images.unsplash.com/photo-1493976040374-85c8e12f0c0e?w=600'],
      address: '京都府京都市北区金阁寺町1',
      openingHours: '09:00-17:00',
      duration: 60,
      price: 500,
      rating: 4.7,
      tags: ['寺庙', '世界遗产', '文化'],
      bestTimeToVisit: '上午',
      location: { lat: 35.0394, lng: 135.7292 },
    },
    {
      id: 'kyoto-2',
      name: '清水寺',
      description: '悬空的木质舞台，俯瞰京都',
      images: ['https://images.unsplash.com/photo-1480796927426-f609979314bd?w=600'],
      address: '京都府京都市东山区清水1-294',
      openingHours: '06:00-18:00',
      duration: 90,
      price: 400,
      rating: 4.6,
      tags: ['寺庙', '世界遗产', '观景'],
      bestTimeToVisit: '上午',
      location: { lat: 34.9949, lng: 135.7850 },
    },
    {
      id: 'kyoto-3',
      name: '伏见稻荷大社',
      description: '千本鸟居，神秘的红色隧道',
      images: ['https://images.unsplash.com/photo-1478436127897-769e1b3f0f36?w=600'],
      address: '京都府京都市伏见区深草薮之内町68',
      openingHours: '全天',
      duration: 120,
      price: 0,
      rating: 4.8,
      tags: ['神社', '世界遗产', '拍照'],
      bestTimeToVisit: '清晨',
      location: { lat: 34.9671, lng: 135.7727 },
    },
    {
      id: 'kyoto-4',
      name: '岚山竹林',
      description: '幽静的竹林小径',
      images: ['https://images.unsplash.com/photo-1545569341-9eb8b30979d9?w=600'],
      address: '京都府京都市右京区嵯峨野',
      openingHours: '08:00-17:00',
      duration: 45,
      price: 0,
      rating: 4.4,
      tags: ['自然', '竹林', '宁静'],
      bestTimeToVisit: '上午',
      location: { lat: 35.0169, lng: 135.6789 },
    },
    {
      id: 'kyoto-5',
      name: '花见小路',
      description: '传统的花街，可能邂逅艺伎',
      images: ['https://images.unsplash.com/photo-1528360983277-13d401cdc186?w=600'],
      address: '京都府京都市东山区祇园町',
      openingHours: '全天',
      duration: 60,
      price: 0,
      rating: 4.3,
      tags: ['传统', '花街', '散步'],
      bestTimeToVisit: '傍晚',
      location: { lat: 35.0037, lng: 135.7753 },
    },
  ],
  osaka: [
    {
      id: 'osaka-1',
      name: '大阪城',
      description: '雄伟的历史名城',
      images: ['https://images.unsplash.com/photo-1590559899731-a382839e5549?w=600'],
      address: '大阪府大阪市中央区大阪城1-1',
      openingHours: '09:00-17:00',
      duration: 90,
      price: 600,
      rating: 4.4,
      tags: ['城堡', '历史', '公园'],
      bestTimeToVisit: '上午',
      location: { lat: 34.6873, lng: 135.5260 },
    },
    {
      id: 'osaka-2',
      name: '道顿堀',
      description: '大阪最热闹的美食街',
      images: ['https://images.unsplash.com/photo-1590559899731-a382839e5549?w=600'],
      address: '大阪府大阪市中央区道顿堀',
      openingHours: '全天',
      duration: 120,
      price: 0,
      rating: 4.3,
      tags: ['美食', '购物', '夜景'],
      bestTimeToVisit: '晚上',
      location: { lat: 34.6698, lng: 135.5015 },
    },
    {
      id: 'osaka-3',
      name: '通天阁',
      description: '大阪的地标塔',
      images: ['https://images.unsplash.com/photo-1590559899731-a382839e5549?w=600'],
      address: '大阪府大阪市浪速区惠比须东1-18-6',
      openingHours: '09:00-21:00',
      duration: 45,
      price: 700,
      rating: 4.0,
      tags: ['观景台', '地标'],
      bestTimeToVisit: '傍晚',
      location: { lat: 34.6525, lng: 135.5055 },
    },
    {
      id: 'osaka-4',
      name: '心斋桥',
      description: '大阪最繁华的购物区',
      images: ['https://images.unsplash.com/photo-1441986300917-64674bd600d8?w=600'],
      address: '大阪府大阪市中央区心斋桥',
      openingHours: '10:00-20:00',
      duration: 120,
      price: 0,
      rating: 4.2,
      tags: ['购物', '时尚'],
      bestTimeToVisit: '下午',
      location: { lat: 34.6745, lng: 135.5000 },
    },
  ],
}
```

- [ ] **Step 3: 创建 src/data/restaurants.js**

```javascript
export const restaurants = {
  tokyo: [
    {
      id: 'tokyo-r-1',
      name: '筑地寿司清',
      description: '新鲜的江户前寿司',
      images: ['https://images.unsplash.com/photo-1579584425555-c3ce17fd4351?w=600'],
      cuisine: '寿司',
      recommendedDishes: ['特选寿司拼盘', '金枪鱼大脂'],
      priceRange: '¥¥¥',
      rating: 4.6,
      address: '东京都中央区筑地4-13-8',
      openingHours: '11:00-22:00',
      tags: ['午餐', '晚餐', '寿司'],
      location: { lat: 35.6654, lng: 139.7696 },
    },
    {
      id: 'tokyo-r-2',
      name: '一兰拉面 新宿',
      description: '经典的豚骨拉面，个人隔间',
      images: ['https://images.unsplash.com/photo-1569718212165-3a8278d5f624?w=600'],
      cuisine: '拉面',
      recommendedDishes: ['原味豚骨拉面', '替玉'],
      priceRange: '¥¥',
      rating: 4.4,
      address: '东京都涩谷区神宫前6-12-15',
      openingHours: '24小时',
      tags: ['夜宵', '拉面', '单人'],
      location: { lat: 35.6696, lng: 139.7006 },
    },
    {
      id: 'tokyo-r-3',
      name: '鸟贵族 涩谷',
      description: '平价串烧连锁店',
      images: ['https://images.unsplash.com/photo-1555939594-58d7cb561ad1?w=600'],
      cuisine: '串烧',
      recommendedDishes: ['烤鸡皮', '烤葱肉串', '烤鸡翅'],
      priceRange: '¥',
      rating: 4.1,
      address: '东京都涩谷区道玄坂2-29-1',
      openingHours: '17:00-05:00',
      tags: ['晚餐', '夜宵', '串烧'],
      location: { lat: 35.6580, lng: 139.6990 },
    },
    {
      id: 'tokyo-r-4',
      name: '天ぷら 近藤',
      description: '酥脆的天妇罗名店',
      images: ['https://images.unsplash.com/photo-1569718212165-3a8278d5f624?w=600'],
      cuisine: '天妇罗',
      recommendedDishes: ['天妇罗拼盘', '炸虾', '炸紫苏叶'],
      priceRange: '¥¥¥',
      rating: 4.5,
      address: '东京都中央区银座6-7-2',
      openingHours: '11:30-14:30, 17:30-21:00',
      tags: ['午餐', '晚餐', '天妇罗'],
      location: { lat: 35.6690, lng: 139.7640 },
    },
  ],
  kyoto: [
    {
      id: 'kyoto-r-1',
      name: ' Nakamura Tokichi 京都站',
      description: '精致的抹茶甜品',
      images: ['https://images.unsplash.com/photo-1558326567-98ae2405596b?w=600'],
      cuisine: '抹茶甜品',
      recommendedDishes: ['抹茶冰淇淋', '抹茶巴菲', '抹茶蕨饼'],
      priceRange: '¥¥',
      rating: 4.5,
      address: '京都府京都市下京区乌丸通塩小路下ル东塩小路町901',
      openingHours: '10:00-20:00',
      tags: ['下午茶', '甜品', '抹茶'],
      location: { lat: 34.9855, lng: 135.7586 },
    },
    {
      id: 'kyoto-r-2',
      name: '京都 嵯峨野 广川',
      description: '岚山著名的鳗鱼饭',
      images: ['https://images.unsplash.com/photo-1579584425555-c3ce17fd4351?w=600'],
      cuisine: '鳗鱼饭',
      recommendedDishes: ['鳗鱼饭', '鳗鱼重'],
      priceRange: '¥¥¥',
      rating: 4.4,
      address: '京都府京都市右京区嵯峨天龙寺北造路町44-1',
      openingHours: '11:30-14:30, 17:00-20:00',
      tags: ['午餐', '晚餐', '鳗鱼'],
      location: { lat: 35.0141, lng: 135.6773 },
    },
    {
      id: 'kyoto-r-3',
      name: '饺子的王将 四条',
      description: '京都起家的煎饺连锁店',
      images: ['https://images.unsplash.com/photo-1563245372-f21724e3856d?w=600'],
      cuisine: '饺子',
      recommendedDishes: ['煎饺', '炒饭', '拉面'],
      priceRange: '¥',
      rating: 4.0,
      address: '京都府京都市下京区四条通乌丸西入ル',
      openingHours: '11:00-23:00',
      tags: ['午餐', '晚餐', '饺子'],
      location: { lat: 35.0037, lng: 135.7612 },
    },
    {
      id: 'kyoto-r-4',
      name: '本家 尾张屋',
      description: '传统的京都荞麦面',
      images: ['https://images.unsplash.com/photo-1569718212165-3a8278d5f624?w=600'],
      cuisine: '荞麦面',
      recommendedDishes: ['笼屉荞麦面', '天妇罗荞麦面'],
      priceRange: '¥¥',
      rating: 4.3,
      address: '京都府京都市东山区祇园町北侧',
      openingHours: '11:00-21:00',
      tags: ['午餐', '晚餐', '荞麦面'],
      location: { lat: 35.0041, lng: 135.7764 },
    },
  ],
  osaka: [
    {
      id: 'osaka-r-1',
      name: '道顿堀 くら寿司',
      description: '平价回转寿司',
      images: ['https://images.unsplash.com/photo-1579584425555-c3ce17fd4351?w=600'],
      cuisine: '寿司',
      recommendedDishes: ['三文鱼寿司', '金枪鱼寿司', '甜虾寿司'],
      priceRange: '¥',
      rating: 4.1,
      address: '大阪府大阪市中央区道顿堀1-7-21',
      openingHours: '11:00-23:00',
      tags: ['午餐', '晚餐', '寿司'],
      location: { lat: 34.6689, lng: 135.5025 },
    },
    {
      id: 'osaka-r-2',
      name: '大阪王将 道顿堀',
      description: '大阪名物煎饺',
      images: ['https://images.unsplash.com/photo-1563245372-f21724e3856d?w=600'],
      cuisine: '饺子',
      recommendedDishes: ['煎饺', '炒饭', '天津饭'],
      priceRange: '¥',
      rating: 4.0,
      address: '大阪府大阪市中央区道顿堀1-6-10',
      openingHours: '11:00-23:00',
      tags: ['午餐', '晚餐', '饺子'],
      location: { lat: 34.6693, lng: 135.5018 },
    },
    {
      id: 'osaka-r-3',
      name: 'たこ家道顿堀 くくる',
      description: '道顿堀著名的章鱼烧',
      images: ['https://images.unsplash.com/photo-1569718212165-3a8278d5f624?w=600'],
      cuisine: '章鱼烧',
      recommendedDishes: ['原味章鱼烧', '明太子章鱼烧'],
      priceRange: '¥',
      rating: 4.2,
      address: '大阪府大阪市中央区道顿堀1-5-10',
      openingHours: '10:00-22:00',
      tags: ['小吃', '章鱼烧'],
      location: { lat: 34.6697, lng: 135.5012 },
    },
    {
      id: 'osaka-r-4',
      name: '自由轩 难波',
      description: '大阪特色咖喱饭',
      images: ['https://images.unsplash.com/photo-1579584425555-c3ce17fd4351?w=600'],
      cuisine: '咖喱饭',
      recommendedDishes: ['原味咖喱饭', '芝士咖喱饭'],
      priceRange: '¥¥',
      rating: 4.1,
      address: '大阪府大阪市中央区难波3-1-34',
      openingHours: '11:30-20:30',
      tags: ['午餐', '晚餐', '咖喱'],
      location: { lat: 34.6645, lng: 135.5005 },
    },
  ],
}
```

---

### Task 4: 创建 Pinia 状态管理

**Files:**
- Create: `src/stores/itinerary.js`

- [ ] **Step 1: 创建 src/stores/itinerary.js**

```javascript
import { defineStore } from 'pinia'
import { ref, computed } from 'vue'

export const useItineraryStore = defineStore('itinerary', () => {
  const selectedDestination = ref(null)
  const travelDays = ref(5)
  const preferences = ref({
    attractionTypes: [],
    foodPreferences: [],
    pace: 'moderate',
    budget: 'medium',
  })
  const currentItinerary = ref(null)

  const setDestination = (destination) => {
    selectedDestination.value = destination
  }

  const setTravelDays = (days) => {
    travelDays.value = days
  }

  const setPreferences = (prefs) => {
    preferences.value = { ...preferences.value, ...prefs }
  }

  const setItinerary = (itinerary) => {
    currentItinerary.value = itinerary
  }

  const clearItinerary = () => {
    currentItinerary.value = null
  }

  return {
    selectedDestination,
    travelDays,
    preferences,
    currentItinerary,
    setDestination,
    setTravelDays,
    setPreferences,
    setItinerary,
    clearItinerary,
  }
})
```

---

### Task 5: 创建线路生成工具

**Files:**
- Create: `src/utils/itineraryGenerator.js`

- [ ] **Step 1: 创建 src/utils/itineraryGenerator.js**

```javascript
import { attractions } from '../data/attractions'
import { restaurants } from '../data/restaurants'

export function generateItinerary(destinationId, days, preferences = {}) {
  const destinationAttractions = attractions[destinationId] || []
  const destinationRestaurants = restaurants[destinationId] || []

  const daysPlan = []

  for (let day = 1; day <= days; day++) {
    const dayActivities = generateDayPlan(
      destinationAttractions,
      destinationRestaurants,
      day,
      preferences
    )
    daysPlan.push({ day, activities: dayActivities })
  }

  return {
    id: Date.now().toString(),
    destination: destinationId,
    days,
    daysPlan,
  }
}

function generateDayPlan(attractionsList, restaurantsList, day, preferences) {
  const activities = []

  const shuffledAttractions = [...attractionsList].sort(() => Math.random() - 0.5)
  const shuffledRestaurants = [...restaurantsList].sort(() => Math.random() - 0.5)

  const selectedAttractions = shuffledAttractions.slice(0, 3)
  const selectedRestaurants = shuffledRestaurants.slice(0, 3)

  if (selectedAttractions[0]) {
    activities.push({
      time: '09:00',
      place: selectedAttractions[0],
      type: 'attraction',
    })
  }

  const lunchPlace = selectedRestaurants.find(r => r.tags.includes('午餐')) || selectedRestaurants[0]
  if (lunchPlace) {
    activities.push({
      time: '12:00',
      place: lunchPlace,
      type: 'meal',
    })
  }

  if (selectedAttractions[1]) {
    activities.push({
      time: '14:00',
      place: selectedAttractions[1],
      type: 'attraction',
    })
  }

  if (selectedAttractions[2]) {
    activities.push({
      time: '16:00',
      place: selectedAttractions[2],
      type: 'attraction',
    })
  }

  const dinnerPlace = selectedRestaurants.find(r => r.tags.includes('晚餐')) || selectedRestaurants[1] || selectedRestaurants[0]
  if (dinnerPlace) {
    activities.push({
      time: '18:30',
      place: dinnerPlace,
      type: 'meal',
    })
  }

  return activities
}
```

---

### Task 6: 创建基础组件

**Files:**
- Create: `src/components/PlaceCard.vue`
- Create: `src/components/DestinationSelector.vue`
- Create: `src/components/FilterPanel.vue`

- [ ] **Step 1: 创建 src/components/PlaceCard.vue**

```vue
<template>
  <div class="bg-white rounded-lg shadow-md overflow-hidden hover:shadow-lg transition-shadow">
    <img :src="place.images[0]" :alt="place.name" class="w-full h-48 object-cover" />
    <div class="p-4">
      <h3 class="text-lg font-semibold text-gray-900">{{ place.name }}</h3>
      <p class="text-sm text-gray-600 mt-1 line-clamp-2">{{ place.description }}</p>
      <div class="flex items-center justify-between mt-3">
        <div class="flex items-center">
          <span class="text-yellow-500">★</span>
          <span class="ml-1 text-sm text-gray-600">{{ place.rating }}</span>
        </div>
        <span class="text-sm text-gray-500">{{ place.priceRange || (place.price === 0 ? '免费' : '¥' + place.price) }}</span>
      </div>
      <div class="flex flex-wrap gap-1 mt-2">
        <span v-for="tag in place.tags.slice(0, 3)" :key="tag" class="px-2 py-1 bg-blue-50 text-blue-600 text-xs rounded">
          {{ tag }}
        </span>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  place: {
    type: Object,
    required: true,
  },
})
</script>
```

- [ ] **Step 2: 创建 src/components/DestinationSelector.vue**

```vue
<template>
  <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
    <div
      v-for="dest in destinations"
      :key="dest.id"
      @click="selectDestination(dest)"
      :class="[
        'cursor-pointer rounded-xl overflow-hidden shadow-md hover:shadow-xl transition-all',
        selected?.id === dest.id ? 'ring-4 ring-blue-500' : ''
      ]"
    >
      <img :src="dest.image" :alt="dest.name" class="w-full h-48 object-cover" />
      <div class="p-4 bg-white">
        <h3 class="text-xl font-bold text-gray-900">{{ dest.name }}</h3>
        <p class="text-gray-600 text-sm mt-1">{{ dest.country }}</p>
        <p class="text-gray-500 text-sm mt-2">{{ dest.description }}</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { destinations } from '../data/destinations'

const emit = defineEmits(['select'])

const selected = defineProps({
  modelValue: Object,
})

const selectDestination = (dest) => {
  emit('update:modelValue', dest)
  emit('select', dest)
}
</script>
```

- [ ] **Step 3: 创建 src/components/FilterPanel.vue**

```vue
<template>
  <div class="bg-white rounded-xl shadow-md p-6">
    <h3 class="text-lg font-semibold mb-4">设置偏好</h3>

    <div class="mb-6">
      <label class="block text-sm font-medium text-gray-700 mb-2">旅行天数</label>
      <div class="flex gap-2">
        <button
          v-for="day in [4, 5, 6, 7]"
          :key="day"
          @click="updateDays(day)"
          :class="[
            'px-4 py-2 rounded-lg border',
            days === day
              ? 'bg-blue-600 text-white border-blue-600'
              : 'bg-white text-gray-700 border-gray-300 hover:bg-gray-50'
          ]"
        >
          {{ day }}天
        </button>
      </div>
    </div>

    <div class="mb-6">
      <label class="block text-sm font-medium text-gray-700 mb-2">行程节奏</label>
      <div class="flex gap-2">
        <button
          v-for="pace in paces"
          :key="pace.value"
          @click="updatePreference('pace', pace.value)"
          :class="[
            'px-4 py-2 rounded-lg border flex-1',
            preferences.pace === pace.value
              ? 'bg-blue-600 text-white border-blue-600'
              : 'bg-white text-gray-700 border-gray-300 hover:bg-gray-50'
          ]"
        >
          {{ pace.label }}
        </button>
      </div>
    </div>

    <div class="mb-6">
      <label class="block text-sm font-medium text-gray-700 mb-2">预算范围</label>
      <div class="flex gap-2">
        <button
          v-for="budget in budgets"
          :key="budget.value"
          @click="updatePreference('budget', budget.value)"
          :class="[
            'px-4 py-2 rounded-lg border flex-1',
            preferences.budget === budget.value
              ? 'bg-blue-600 text-white border-blue-600'
              : 'bg-white text-gray-700 border-gray-300 hover:bg-gray-50'
          ]"
        >
          {{ budget.label }}
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { reactive } from 'vue'

const props = defineProps({
  modelValue: {
    type: Object,
    default: () => ({}),
  },
  days: {
    type: Number,
    default: 5,
  },
})

const emit = defineEmits(['update:modelValue', 'update:days'])

const preferences = reactive({
  pace: 'moderate',
  budget: 'medium',
  ...props.modelValue,
})

const paces = [
  { value: 'relaxed', label: '轻松' },
  { value: 'moderate', label: '适中' },
  { value: 'tight', label: '紧凑' },
]

const budgets = [
  { value: 'economy', label: '经济' },
  { value: 'medium', label: '中等' },
  { value: 'luxury', label: '豪华' },
]

const updatePreference = (key, value) => {
  preferences[key] = value
  emit('update:modelValue', { ...preferences })
}

const updateDays = (day) => {
  emit('update:days', day)
}
</script>
```

---

### Task 7: 创建首页

**Files:**
- Create: `src/views/Home.vue`

- [ ] **Step 1: 创建 src/views/Home.vue**

```vue
<template>
  <div class="space-y-8">
    <div class="text-center">
      <h1 class="text-4xl font-bold text-gray-900">规划你的完美旅程</h1>
      <p class="text-xl text-gray-600 mt-4">选择目的地，AI 智能生成专属旅游攻略</p>
    </div>

    <section>
      <h2 class="text-2xl font-semibold mb-4">选择目的地</h2>
      <DestinationSelector v-model="selectedDestination" @select="onSelectDestination" />
    </section>

    <section v-if="selectedDestination" class="animate-fadeIn">
      <h2 class="text-2xl font-semibold mb-4">定制你的行程</h2>
      <FilterPanel
        v-model="preferences"
        v-model:days="travelDays"
      />
    </section>

    <div v-if="selectedDestination" class="text-center">
      <button
        @click="generateItinerary"
        class="px-8 py-4 bg-blue-600 text-white text-lg font-semibold rounded-xl hover:bg-blue-700 transition-colors shadow-lg"
      >
        ✨ 生成线路
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import { useItineraryStore } from '../stores/itinerary'
import { generateItinerary as generateItineraryFn } from '../utils/itineraryGenerator'
import DestinationSelector from '../components/DestinationSelector.vue'
import FilterPanel from '../components/FilterPanel.vue'

const router = useRouter()
const store = useItineraryStore()

const selectedDestination = ref(null)
const travelDays = ref(5)
const preferences = ref({
  pace: 'moderate',
  budget: 'medium',
})

const onSelectDestination = (dest) => {
  store.setDestination(dest)
}

const generateItinerary = () => {
  store.setTravelDays(travelDays.value)
  store.setPreferences(preferences.value)

  const itinerary = generateItineraryFn(
    selectedDestination.value.id,
    travelDays.value,
    preferences.value
  )

  store.setItinerary(itinerary)
  router.push('/itinerary')
}
</script>

<style>
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}
.animate-fadeIn {
  animation: fadeIn 0.5s ease-out;
}
</style>
```

---

### Task 8: 创建线路时间线组件和页面

**Files:**
- Create: `src/components/ItineraryTimeline.vue`
- Create: `src/views/Itinerary.vue`

- [ ] **Step 1: 创建 src/components/ItineraryTimeline.vue**

```vue
<template>
  <div class="space-y-8">
    <div v-for="(dayPlan, dayIndex) in itinerary.daysPlan" :key="dayPlan.day" class="animate-fadeIn">
      <h3 class="text-xl font-bold text-gray-900 mb-4">
        第 {{ dayPlan.day }} 天
      </h3>
      <div class="relative border-l-2 border-blue-200 ml-4 pl-8 space-y-6">
        <div
          v-for="(activity, actIndex) in dayPlan.activities"
          :key="actIndex"
          class="relative"
        >
          <div class="absolute -left-10 w-6 h-6 rounded-full bg-blue-500 flex items-center justify-center">
            <span class="text-white text-xs">
              {{ activity.type === 'attraction' ? '📍' : '🍽️' }}
            </span>
          </div>
          <div class="bg-white rounded-lg shadow-md p-4 hover:shadow-lg transition-shadow">
            <div class="flex items-start justify-between">
              <div class="flex-1">
                <div class="flex items-center gap-2">
                  <span class="text-sm font-mono text-blue-600 bg-blue-50 px-2 py-1 rounded">
                    {{ activity.time }}
                  </span>
                  <span class="text-xs text-gray-500">
                    {{ activity.type === 'attraction' ? '景点' : '美食' }}
                  </span>
                </div>
                <h4 class="text-lg font-semibold text-gray-900 mt-2">
                  {{ activity.place.name }}
                </h4>
                <p class="text-sm text-gray-600 mt-1">
                  {{ activity.place.description }}
                </p>
                <div class="flex items-center gap-4 mt-2 text-sm text-gray-500">
                  <span>★ {{ activity.place.rating }}</span>
                  <span v-if="activity.place.address">{{ activity.place.address }}</span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  itinerary: {
    type: Object,
    required: true,
  },
})
</script>

<style>
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}
.animate-fadeIn {
  animation: fadeIn 0.4s ease-out;
}
</style>
```

- [ ] **Step 2: 创建 src/views/Itinerary.vue**

```vue
<template>
  <div v-if="!itinerary" class="text-center py-20">
    <p class="text-xl text-gray-600">还没有生成线路，请先选择目的地</p>
    <router-link to="/" class="inline-block mt-4 text-blue-600 hover:underline">
      返回首页
    </router-link>
  </div>

  <div v-else class="space-y-6">
    <div class="flex items-center justify-between">
      <div>
        <h1 class="text-3xl font-bold text-gray-900">
          {{ destinationName }} {{ itinerary.days }} 天攻略
        </h1>
        <p class="text-gray-600 mt-1">
          为你精心定制的旅行线路
        </p>
      </div>
      <div class="flex gap-3">
        <button
          @click="regenerate"
          class="px-4 py-2 border border-gray-300 rounded-lg text-gray-700 hover:bg-gray-50"
        >
          🔄 重新生成
        </button>
        <router-link
          to="/"
          class="px-4 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700"
        >
          🏠 返回首页
        </router-link>
      </div>
    </div>

    <ItineraryTimeline :itinerary="itinerary" />
  </div>
</template>

<script setup>
import { computed } from 'vue'
import { useRouter } from 'vue-router'
import { useItineraryStore } from '../stores/itinerary'
import { destinations } from '../data/destinations'
import { generateItinerary } from '../utils/itineraryGenerator'
import ItineraryTimeline from '../components/ItineraryTimeline.vue'

const router = useRouter()
const store = useItineraryStore()

const itinerary = computed(() => store.currentItinerary)

const destinationName = computed(() => {
  if (!itinerary.value) return ''
  const dest = destinations.find(d => d.id === itinerary.value.destination)
  return dest?.name || ''
})

const regenerate = () => {
  if (!store.selectedDestination) {
    router.push('/')
    return
  }
  const newItinerary = generateItinerary(
    store.selectedDestination.id,
    store.travelDays,
    store.preferences
  )
  store.setItinerary(newItinerary)
}
</script>
```

---

### Task 9: 创建详情页面

**Files:**
- Create: `src/views/AttractionDetail.vue`
- Create: `src/views/RestaurantDetail.vue`

- [ ] **Step 1: 创建 src/views/AttractionDetail.vue**

```vue
<template>
  <div v-if="!attraction" class="text-center py-20">
    <p class="text-xl text-gray-600">景点不存在</p>
    <router-link to="/" class="inline-block mt-4 text-blue-600 hover:underline">
      返回首页
    </router-link>
  </div>

  <div v-else class="max-w-4xl mx-auto">
    <button @click="$router.back()" class="text-blue-600 hover:underline mb-6">
      ← 返回
    </button>

    <div class="bg-white rounded-xl shadow-md overflow-hidden">
      <img :src="attraction.images[0]" :alt="attraction.name" class="w-full h-80 object-cover" />
      <div class="p-8">
        <h1 class="text-3xl font-bold text-gray-900">{{ attraction.name }}</h1>

        <div class="flex items-center gap-4 mt-4">
          <div class="flex items-center">
            <span class="text-yellow-500 text-xl">★</span>
            <span class="ml-1 text-lg font-semibold">{{ attraction.rating }}</span>
          </div>
          <span class="text-gray-500">{{ attraction.price === 0 ? '免费' : '¥' + attraction.price }}</span>
        </div>

        <p class="text-gray-700 mt-6 text-lg">{{ attraction.description }}</p>

        <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mt-8">
          <div>
            <h3 class="font-semibold text-gray-900 mb-2">📍 地址</h3>
            <p class="text-gray-600">{{ attraction.address }}</p>
          </div>
          <div>
            <h3 class="font-semibold text-gray-900 mb-2">🕐 开放时间</h3>
            <p class="text-gray-600">{{ attraction.openingHours }}</p>
          </div>
          <div>
            <h3 class="font-semibold text-gray-900 mb-2">⏱️ 建议游玩时长</h3>
            <p class="text-gray-600">{{ attraction.duration }} 分钟</p>
          </div>
          <div>
            <h3 class="font-semibold text-gray-900 mb-2">🌟 最佳游览时间</h3>
            <p class="text-gray-600">{{ attraction.bestTimeToVisit }}</p>
          </div>
        </div>

        <div class="mt-8">
          <h3 class="font-semibold text-gray-900 mb-3">标签</h3>
          <div class="flex flex-wrap gap-2">
            <span
              v-for="tag in attraction.tags"
              :key="tag"
              class="px-3 py-1 bg-blue-50 text-blue-600 rounded-full"
            >
              {{ tag }}
            </span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'
import { useRoute } from 'vue-router'
import { attractions } from '../data/attractions'

const route = useRoute()

const attraction = computed(() => {
  const id = route.params.id
  for (const key in attractions) {
    const found = attractions[key].find(a => a.id === id)
    if (found) return found
  }
  return null
})
</script>
```

- [ ] **Step 2: 创建 src/views/RestaurantDetail.vue**

```vue
<template>
  <div v-if="!restaurant" class="text-center py-20">
    <p class="text-xl text-gray-600">餐厅不存在</p>
    <router-link to="/" class="inline-block mt-4 text-blue-600 hover:underline">
      返回首页
    </router-link>
  </div>

  <div v-else class="max-w-4xl mx-auto">
    <button @click="$router.back()" class="text-blue-600 hover:underline mb-6">
      ← 返回
    </button>

    <div class="bg-white rounded-xl shadow-md overflow-hidden">
      <img :src="restaurant.images[0]" :alt="restaurant.name" class="w-full h-80 object-cover" />
      <div class="p-8">
        <h1 class="text-3xl font-bold text-gray-900">{{ restaurant.name }}</h1>

        <div class="flex items-center gap-4 mt-4">
          <div class="flex items-center">
            <span class="text-yellow-500 text-xl">★</span>
            <span class="ml-1 text-lg font-semibold">{{ restaurant.rating }}</span>
          </div>
          <span class="text-gray-500">{{ restaurant.priceRange }}</span>
          <span class="text-gray-500">{{ restaurant.cuisine }}</span>
        </div>

        <p class="text-gray-700 mt-6 text-lg">{{ restaurant.description }}</p>

        <div class="mt-8">
          <h3 class="font-semibold text-gray-900 mb-3">🍽️ 推荐菜品</h3>
          <div class="flex flex-wrap gap-2">
            <span
              v-for="dish in restaurant.recommendedDishes"
              :key="dish"
              class="px-3 py-1 bg-orange-50 text-orange-600 rounded-full"
            >
              {{ dish }}
            </span>
          </div>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mt-8">
          <div>
            <h3 class="font-semibold text-gray-900 mb-2">📍 地址</h3>
            <p class="text-gray-600">{{ restaurant.address }}</p>
          </div>
          <div>
            <h3 class="font-semibold text-gray-900 mb-2">🕐 营业时间</h3>
            <p class="text-gray-600">{{ restaurant.openingHours }}</p>
          </div>
        </div>

        <div class="mt-8">
          <h3 class="font-semibold text-gray-900 mb-3">标签</h3>
          <div class="flex flex-wrap gap-2">
            <span
              v-for="tag in restaurant.tags"
              :key="tag"
              class="px-3 py-1 bg-blue-50 text-blue-600 rounded-full"
            >
              {{ tag }}
            </span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'
import { useRoute } from 'vue-router'
import { restaurants } from '../data/restaurants'

const route = useRoute()

const restaurant = computed(() => {
  const id = route.params.id
  for (const key in restaurants) {
    const found = restaurants[key].find(r => r.id === id)
    if (found) return found
  }
  return null
})
</script>
```

---

### Task 10: 安装依赖并测试运行

**Files:**
- Modify: (安装依赖，运行项目)

- [ ] **Step 1: 安装依赖**

```bash
npm install
```

- [ ] **Step 2: 启动开发服务器**

```bash
npm run dev
```

- [ ] **Step 3: 构建生产版本**

```bash
npm run build
```

---

## 计划自检

1. ✅ **规格覆盖**: 所有规格要求都有对应的任务实现
2. ✅ **占位符检查**: 无占位符，所有步骤都有完整代码
3. ✅ **类型一致**: 所有类型、函数名、属性名前后一致
