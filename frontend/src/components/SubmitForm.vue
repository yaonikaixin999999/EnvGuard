<template>
    <div class="modal-backdrop" v-if="showModal" @click.self="close">
      <div class="modal-container">
        <div class="modal-header">
          <h3>提交信息</h3>
          <button class="close-btn" @click="close">✖</button>
        </div>
  
        <form @submit.prevent="submitForm" class="modal-body">
            <table class="air-quality-table">
                <thead>
                    <tr>
                    <th>一</th>
                    <th>优</th>
                    <th>空气质量令人满意，基本无空气污染</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                    <td>二</td>
                    <td>良</td>
                    <td>空气质量可接受，但某些污染物可能对极少数异常敏感人群健康有较弱影响</td>
                    </tr>
                    <tr>
                    <td>三</td>
                    <td>轻度污染</td>
                    <td>易感人群症状有轻度加剧，健康人群可能出现刺激症状</td>
                    </tr>
                    <tr>
                    <td>四</td>
                    <td>中度污染</td>
                    <td>进一步加剧易感人群症状，可能对健康人群心脏、呼吸系统有影响</td>
                    </tr>
                    <tr>
                    <td>五</td>
                    <td>重度污染</td>
                    <td>心脏病和肺病患者症状显著加剧，运动耐受力降低，健康人群普遍出现症状</td>
                    </tr>
                    <tr>
                    <td>六</td>
                    <td>严重污染</td>
                    <td>健康人群运动耐受力降低，有明显强烈症状，提前出现某些疾病</td>
                    </tr>
                </tbody>
            </table>

            <textarea v-model="formData.feedback" placeholder="请输入反馈信息" required></textarea>
          <div class="modal-footer">
            <button type="button" @click="close">取消</button>
            <button type="submit">提交</button>
          </div>
        </form>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref,defineProps } from 'vue'
  
  const props = defineProps({
    address: {
      type: String,
      default: ''
    },
    selectedProvince: {
      type: String,
      default: ''
    },
    selectedCity: {
      type: String,
      default: ''
    },
  })
    const showModal = ref(false)
    const formData = ref({
        feedback: ''
  })
  
  
  // 控制弹窗显示和隐藏
  function open() {
    showModal.value = true
  }
  
  function close() {
    showModal.value = false
    // 可选：重置表单
    formData.value = {
      feedback: ''
    }
  }
  
  // 表单提交处理
  function submitForm() {
    if (!formData.value.feedback) {
      alert('请填写完整信息')
      return
    }
  
    console.log('提交的数据:', formData.value)
  
    // 这里可以调用 API 或 emit 给父组件
    // 示例：使用 window.alert 显示成功
    alert('提交成功！')
    close()
  }
  
  // 导出方法供父组件调用
  defineExpose({ open, close })
  </script>
  
  <style scoped>
  @import '@/assets/css/SubmitForm.css';
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