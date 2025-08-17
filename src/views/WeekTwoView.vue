<template>
  <h2>註冊</h2>
  <input type="email" placeholder="請輸入email" v-model="signupTemplate.email" />
  <input type="password" placeholder="請輸入密碼" v-model="signupTemplate.password" />
  <input type="text" placeholder="請輸入暱稱" v-model="signupTemplate.nickname" />
  <br />
  {{ signupTemplate }}
  <br />
  <button type="button" @click="signup">註冊</button>
  <br />
  {{ signupRes }}

  <h2>登入</h2>
  <input type="email" placeholder="請輸入email" v-model="signinTemplate.email" />
  <input type="password" placeholder="請輸入密碼" v-model="signinTemplate.password" />
  <br />
  {{ signinTemplate }}
  <br />
  <button type="button" @click="signin">登入</button>
  <br />
  <!-- 登入成功會給一組token -->
  <p>Token:{{ signinRes }}</p>
  <br />
  <h2>驗證</h2>
  <div v-if="user.uid">
    <p>UID:{{ user.uid }}</p>
    <p>NickName:{{ user.nickname }}</p>
  </div>
  <div v-else>你還沒有登入</div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';

// API 基礎路徑
const api = 'https://todolist-api.hexschool.io/';

// 處理註冊功能
// 註冊表單資料
// 使用 ref 建立響應式物件，用於 v-model 雙向綁定
const signupRes = ref('');

const signupTemplate = ref({
  email: '',
  password: '',
  nickname: '',
});

const signup = async () => {
  try {
    // 發送 POST 請求至註冊 API
    // signupTemplate.value 會被轉換為 JSON 格式的請求主體 (request body)
    const res = await axios.post(`${api}users/sign_up`, signupTemplate.value);

    // 成功時，印出完整的回應物件
    console.log('註冊成功！', res);

    // 取得註冊成功的uid
    signupRes.value = res.data.uid;
  } catch (error) {
    // 捕獲錯誤，並印出詳細資訊
    // error.response 包含伺服器回傳的錯誤內容和狀態碼
    if (error.response) {
      console.log('伺服器錯誤回應:', error.response.data);
      console.log('HTTP 狀態碼:', error.response.status);
    } else {
      console.log('Axios 請求錯誤:', error.message);
    }
  }
};

// 處理登入功能
const signinRes = ref('');

const signinTemplate = ref({
  email: '',
  password: '',
});

const signin = async () => {
  try {
    // 發送 POST 請求至註冊 API
    // signinTemplate.value 會被轉換為 JSON 格式的請求主體 (request body)
    const res = await axios.post(`${api}users/sign_in`, signinTemplate.value);

    // 成功時，印出完整的回應物件
    console.log('登入成功！', res);

    // 取得登入成功的token
    signinRes.value = res.data.token;

    // 製作cookie
    document.cookie = `myCookie=${res.data.token};path=/`;
  } catch (error) {
    // 捕獲錯誤，並印出詳細資訊
    // error.response 包含伺服器回傳的錯誤內容和狀態碼
    if (error.response) {
      console.log('伺服器錯誤回應:', error.response.data);
      console.log('HTTP 狀態碼:', error.response.status);
    } else {
      console.log('Axios 請求錯誤:', error.message);
    }
  }
};

// 製作驗證功能
const user = ref({
  nickname: '',
  uid: '',
});

// onMounted 內設定驗證
onMounted(async () => {
  const token = document.cookie.replace(/(?:^|.*;\s*)myCookie\s*=\s*([^;]*).*$/i, '$1');
  console.log(token);
  const res = await axios.get(`${api}users/checkout`, {
    headers: {
      Authorization: token,
    },
  });
  console.log(res);
  user.value = res.data;
});
</script>
