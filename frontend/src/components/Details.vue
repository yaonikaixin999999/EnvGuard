<template>
    <div class="modal-backdrop" @click.self="close">
      <div class="modal-container">
        <div class="modal-header">
          <h3>提交信息</h3>
          <button class="close-btn" @click="close">✖</button>
        </div>
        <form @submit.prevent="submitForm" class="modal-body">
            <h2>详细信息</h2>
            <p><strong>标识符:</strong> {{ props.item.label }}</p>
            <p><strong>省份:</strong> {{ props.item.province }}</p>
            <p><strong>城市:</strong> {{ props.item.city }}</p>
            <p><strong>日期:</strong> {{ props.item.date }}</p>
            <p><strong>地址:</strong> {{ props.item.address }}</p>
            <p><strong>SO2浓度:</strong> <input type="text" on-model="sub_api.SO2">ug/m3</p>
            <p><strong>CO浓度:</strong> <input type="text" on-model="sub_api.SO2">ug/m3</p>
            <p><strong>PM2.5浓度:</strong> <input type="text" on-model="sub_api.SO2">ug/m3</p>
            <button type="submit"@click="submit">提交实测数据</button>
            <button type="button"@click="close" class="close-button">关闭详情</button>  
            <p>实测</p>
        </form>
      </div>
    </div>
</template>
  
<script setup>
  import { ref,defineProps,defineEmits } from 'vue';
  

  function sumit(){
    
  }
  // 定义组件接收的 props
  const props = defineProps({
    item: {
      type: Object, // 期待传入一个对象
      default: null // 默认值为 null
    }
  });

  const sub_api = ref({
    api_id : '',
    SO2: '',
    CO:'',
    spm:'',
  })
  
  const emit = defineEmits(['close']);

  function close(){
    emit('close')
  }

</script>
  
  <style scoped>
  .modal-backdrop {
    position: fixed;
    top: 0; left: 0; right: 0; bottom: 0;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 1000;
  }
  
  .modal-container {
    background: white;
    padding: 20px;
    border-radius: 8px;
    width: 400px;
    position: relative;
  }
  
  .modal-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 15px;
  }
  
  .close-btn {
    background: none;
    border: none;
    font-size: 18px;
    cursor: pointer;
  }
  
  .form-group {
    margin-bottom: 15px;
  }
  
  label {
    display: block;
    margin-bottom: 5px;
  }
  
  input {
    width: 100%;
    padding: 8px;
    box-sizing: border-box;
  }
  
  .modal-footer {
    display: flex;
    justify-content: flex-end;
    gap: 10px;
  }
  
  button {
    padding: 8px 16px;
  }
  </style>