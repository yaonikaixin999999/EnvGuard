<template>
    <div class="form">
        <h2>地址选择</h2>

        <!-- 所在省 -->
        <label>省份：</label>
        <select v-model="selectedProvince" @change="updateCities">
            <option disabled value="">请选择省份</option>
            <option v-for="(cities, province) in provinces" :key="province" :value="province">
                {{ province }}
            </option>
        </select>

        <!-- 所在市 -->
        <label>城市：</label>
        <select v-model="selectedCity" :disabled="!selectedProvince">
            <option disabled value="">请选择城市</option>
            <option v-for="city in cities" :key="city" :value="city">{{ city }}</option>
        </select>

        <!-- 具体地址 -->
        <label>具体地址：</label>
        <input type = "text" v-model="address" placeholder="点击下方按钮获取当前位置" />

        <button @click="getCurrentAddress">📍 获取当前位置</button>
        <button @click="openModal">打开提交表单</button>
        <SubmitForm 
        ref="model"
        :address="address"
        :selectedProvince="selectedProvince"
        :selectedCity="selectedCity"
        />
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import SubmitForm from '@/components/SubmitForm.vue'
import { nextTick } from 'vue'

const model = ref(null)
function openModal(){
    nextTick(() => {
        if(!selectedCity.value || !selectedProvince.value || !address.value){
            alert('请先选择省市区或获取当前位置')
            return
        }
        model.value.open()
    })
}
// 省市区选择数据
const provinces = {
    "北京市": ["北京市"],
    "广东省": ["广州市", "深圳市", "佛山市"],
    "浙江省": ["杭州市", "宁波市", "温州市"]
}

const selectedProvince = ref('')
const selectedCity = ref('')
const cities = ref([])
const address = ref('')


// 省份变化时更新城市
function updateCities() {
    selectedCity.value = ''
    address.value = ''
    const province = selectedProvince.value
    cities.value = provinces[province] || []
}

// 高德地图 Key
const key = '0455c3c453d0791490b6b01aaa1cf410';

// 获取原始 GPS 坐标
function getGPSPosition() {
    return new Promise((resolve, reject) => {
        if (!navigator.geolocation) {
            reject(new Error('浏览器不支持地理定位'));
        } else {
            navigator.geolocation.getCurrentPosition(resolve, reject, {
                enableHighAccuracy: true,
                timeout: 10000,
                maximumAge: 0
            });
        }
    });
}

// 获取当前地址
async function getCurrentAddress() {
    try {
        // 获取原始GPS坐标（WGS-84）
        const position = await getGPSPosition();
        const { latitude, longitude } = position.coords;
        console.log('原始GPS坐标：', longitude, latitude);

        // 坐标转换：GPS -> GCJ-02
        const locations = `${longitude},${latitude}`;
        const convertUrl = `https://restapi.amap.com/v3/assistant/coordinate/convert?locations=${locations}&coordsys=gps&output=json&key=${key}`;
        const convertRes = await fetch(convertUrl);
        const convertData = await convertRes.json();

        if (convertData.status !== '1') {
            throw new Error('坐标转换失败：' + convertData.info);
        }

        const convertedLoc = convertData.locations;
        console.log('转换后高德坐标：', convertedLoc);

        // 逆地理编码获取地址
        const geoUrl = ` https://restapi.amap.com/v3/geocode/regeo?location=${convertedLoc}&key=${key}`;
        const geoRes = await fetch(geoUrl);
        const geoData = await geoRes.json();

        if (geoData.status !== '1') {
            throw new Error('逆地理编码失败：' + geoData.info);
        }

        // 设置地址信息
        const addr = geoData.regeocode.formatted_address;
        address.value = addr;
        console.log('当前地址：', addr);
        console.log('geoData:', geoData);

        // alert('当前位置是：' + addr);

    } catch (error) {
        console.error('获取地址失败:', error.message);
        alert('获取位置失败：' + error.message);
    }
}

// 页面加载时获取地址
onMounted(() => {
    getCurrentAddress()
})
</script>

<style scoped>
.form {
    display: flex;
    flex-direction: column;
    gap: 8px;
    max-width: 400px;
    margin: 2rem auto;
    font-family: Arial, sans-serif;
}

select,
input,
button {
    padding: 6px;
    font-size: 16px;
}
</style>