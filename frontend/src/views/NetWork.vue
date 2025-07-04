<template>
    <div id="app">
      <h1>检测点信息</h1>
      <div v-if="loading" class="loading">加载中...</div>
      <div v-else-if="error" class="error">加载失败：{{ error }}</div>
      <div v-else class="data-list">
        <div v-for="item in dataList" :key="item.id" class="list-item">
          <div class="icon-section" :style="{ backgroundColor: getColor(item.label) }">
            {{ item.label }}
          </div>
          <div class="info-section">
            <div class="main-info">
              <span class="province-city">{{ item.province }} {{ item.city }}</span>
              <span class="date">{{ item.date }}</span>
            </div>
            <div class="address">{{ item.address }}</div>
          </div>
          <button class="check-button" @click="showModal(item)">去检测 <span>&#9733;</span></button>
    
        </div>
      </div>
      <Details v-if="selectedItem" :item="selectedItem" @close="selectedItem = null" />
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted,nextTick } from 'vue';
  import Details from '@/components/Details.vue'

  const selectedItem = ref(null)
  function showModal(item) {
    console.log('点击了详情按钮，准备打开详情页，发送数据：',item)
    nextTick(() => {
      selectedItem.value = item
    })
  }
  
  // 模拟后端数据
  const mockBackendData = [
    {
      "id": 1,
      "label": "四",
      "province": "辽宁省",
      "city": "沈阳市",
      "date": "2022-09-13",
      "address": "大东区岩泉路456号"
    },
    {
      "id": 2,
      "label": "三",
      "province": "辽宁省",
      "city": "沈阳市",
      "date": "2022-09-13",
      "address": "杏花岭区南八路4-67号"
    },
    {
      "id": 3,
      "label": "五",
      "province": "辽宁省",
      "city": "大连市",
      "date": "2022-09-13",
      "address": "河东区南二路奉义里1-232-6号"
    },
    {
      "id": 4,
      "label": "三",
      "province": "辽宁省",
      "city": "沈阳市",
      "date": "2022-09-14",
      "address": "井陉矿区正定街衡西路"
    }
  ];
  
  const dataList = ref([]);
  const loading = ref(true);
  const error = ref(null);
  
  // 模拟从后端获取数据的方法
  const fetchData = async () => {
    try {
      // 模拟网络延迟
      await new Promise(resolve => setTimeout(resolve, 500));
      dataList.value = mockBackendData; // 使用模拟数据
    } catch (e) {
      error.value = e.message;
    } finally {
      loading.value = false;
    }
  };
  
  // 根据 'label' 获取颜色，这里是一个简单的映射
  const getColor = (label) => {
    switch (label) {
      case '四':
        return '#DC3545'; // 红色
      case '三':
        // 需要区分两种三，或者后端提供更明确的颜色信息
        // 这里根据图片推断，如果后端有提供颜色字段，直接用后端提供的
        return '#FFC107'; // 黄色/橙色
      case '五':
        return '#6F42C1'; // 紫色
      default:
        return '#6C757D'; // 默认灰色
    }
  };
  
  // 组件挂载时获取数据
  onMounted(() => {
    fetchData();
  });
  </script>
  
  <style scoped>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
    margin-top: 20px;
    max-width: 800px;
    margin-left: auto;
    margin-right: auto;
    padding: 0 15px;
  }
  
  h1 {
    text-align: center;
    color: #333;
  }
  
  .loading, .error {
    text-align: center;
    padding: 20px;
    font-size: 1.1em;
    color: #555;
  }
  
  .error {
    color: #DC3545;
  }
  
  .data-list {
    border: 1px solid #eee;
    border-radius: 8px;
    overflow: hidden;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  }
  
  .list-item {
    display: flex;
    align-items: center;
    padding: 15px 20px;
    border-bottom: 1px solid #eee;
    background-color: #fff;
    transition: background-color 0.2s ease-in-out;
  }
  
  .list-item:last-child {
    border-bottom: none;
  }
  
  .list-item:hover {
    background-color: #f9f9f9;
  }
  
  .icon-section {
    flex-shrink: 0; /* 防止图标被压缩 */
    width: 40px;
    height: 40px;
    border-radius: 8px;
    display: flex;
    justify-content: center;
    align-items: center;
    font-weight: bold;
    color: white;
    font-size: 1.2em;
    margin-right: 15px;
  }
  
  .info-section {
    flex-grow: 1; /* 占据剩余空间 */
    display: flex;
    flex-direction: column;
  }
  
  .main-info {
    display: flex;
    justify-content: space-between;
    align-items: baseline;
    margin-bottom: 5px;
  }
  
  .province-city {
    font-weight: bold;
    font-size: 1.1em;
    color: #333;
  }
  
  .date {
    font-size: 0.9em;
    color: #666;
  }
  
  .address {
    font-size: 0.9em;
    color: #777;
    line-height: 1.4;
  }
  
  .check-button {
    flex-shrink: 0; /* 防止按钮被压缩 */
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 5px;
    padding: 8px 15px;
    margin-left: 20px;
    cursor: pointer;
    font-size: 0.95em;
    display: flex;
    align-items: center;
    gap: 5px; /* 按钮内部文字和图标的间距 */
    transition: background-color 0.2s ease-in-out;
  }
  
  .check-button:hover {
    background-color: #0056b3;
  }
  
  .check-button span {
    font-size: 1.2em; /* 星星图标大小 */
  }
  </style>