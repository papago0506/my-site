---
name: Travel Guide Website Design
description: Design specification for a travel guide website with itinerary generation features
type: project
---

# 旅游攻略网站设计规格说明

## 项目概述
创建一个旅游攻略网站，核心功能是帮助用户生成推荐的旅游线路，包括景点介绍、美食推荐等功能。

## 技术架构

### 技术栈选择
- **框架**: Vue 3 (Composition API)
- **构建工具**: Vite
- **路由**: Vue Router 4
- **状态管理**: Pinia
- **样式**: Tailwind CSS
- **部署**: Vercel

### 项目结构
```
my-site/
├── src/
│   ├── components/       # 可复用组件
│   ├── views/           # 页面组件
│   ├── stores/          # Pinia状态管理
│   ├── data/            # 静态数据（景点、美食等）
│   ├── router/          # 路由配置
│   └── utils/           # 工具函数
├── public/
└── package.json
```

## 核心功能模块

### 页面结构
1. **首页** - 目的地选择、旅行天数设置、偏好筛选
2. **线路生成页** - 展示推荐线路、可拖拽调整、时间安排
3. **景点详情页** - 景点介绍、图片、开放时间、评分
4. **美食详情页** - 餐厅介绍、推荐菜品、人均消费
5. **我的线路** - 查看已生成的线路、导出分享

### 核心组件
- `DestinationSelector` - 目的地选择器
- `ItineraryTimeline` - 线路时间线展示
- `PlaceCard` - 景点/美食卡片
- `DragDropList` - 可拖拽排序列表
- `FilterPanel` - 筛选偏好面板

## 数据结构设计

### 景点数据 (Attraction)
```javascript
{
  id: string,
  name: string,
  description: string,
  images: string[],
  location: { lat: number, lng: number },
  address: string,
  openingHours: string,
  duration: number, // 建议游玩时长（分钟）
  price: number,
  rating: number,
  tags: string[], // ['文化', '自然', '亲子', ...]
  bestTimeToVisit: string
}
```

### 美食数据 (Restaurant)
```javascript
{
  id: string,
  name: string,
  description: string,
  images: string[],
  cuisine: string, // 菜系
  recommendedDishes: string[],
  priceRange: string, // '¥¥', '¥¥¥'
  rating: number,
  address: string,
  openingHours: string,
  tags: string[] // ['早餐', '正餐', '夜宵', ...]
}
```

### 线路数据 (Itinerary)
```javascript
{
  id: string,
  destination: string,
  days: number,
  daysPlan: [
    {
      day: number,
      activities: [
        {
          time: string, // '09:00'
          place: Attraction | Restaurant,
          type: 'attraction' | 'meal' | 'transport'
        }
      ]
    }
  ]
}
```

## 用户交互流程

### 线路生成主流程
1. 用户选择目的地 → 选择4-7天旅行天数
2. 设置偏好：
   - 景点类型（文化/自然/亲子/购物等）
   - 美食偏好（本地菜/火锅/日料等）
   - 行程节奏（紧凑/适中/轻松）
   - 预算范围（经济/中等/豪华）
3. 点击"生成线路" → 展示推荐线路
4. 用户可调整：
   - 拖拽重新排序活动
   - 替换某个景点/美食
   - 调整时间安排
5. 保存/导出线路（复制链接或截图）

### 线路生成算法逻辑
- 基于用户偏好筛选景点和美食
- 按地理位置聚类，减少交通时间
- 平衡每天的活动量（上午、下午、晚上合理分配）
- 搭配景点+美食的组合
- 避开营业时间冲突

## UI设计方向

### 视觉风格：简约现代
- **主色调**: 蓝色系（#3B82F6 为主色）传达旅行的专业和可靠感
- **辅助色**: 橙色（#F97316）作为强调色，用于按钮和重要操作
- **配色**: 大量留白，浅灰背景，黑色文字，高对比度确保可读性
- **字体**: 无衬线字体，清晰易读
- **间距**: 舒适的8px网格系统，元素间距统一
- **圆角**: 适度的圆角（8px），既现代又友好

### 页面布局
- 顶部导航栏：Logo、主要页面链接
- 内容区域：居中布局，最大宽度1200px
- 卡片设计：景点/美食使用白色卡片，带轻微阴影
- 响应式：适配桌面、平板、手机
