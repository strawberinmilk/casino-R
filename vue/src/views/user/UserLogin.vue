<template>
  <div>
    <div :id="$style.authParent">
      <el-row>
        <el-col :span="8"> メールアドレス </el-col>
        <el-col :span="16">
          <el-input v-model="authData.email" placeholder="sample@example.com"></el-input>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="8"> パスワード </el-col>
        <el-col :span="16">
          <el-input v-model="authData.pass" placeholder="password"></el-input>
        </el-col>
      </el-row>
      <el-button @click="login">ログイン</el-button><br />
      <a href="/user/create">アカウント新規作成はこちら</a>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive } from 'vue';
import { getUserByAuth } from '@/api/user';
import * as notification from '@/components/notification';
// import { useRouter } from 'vue-router';
// const router = useRouter();

export default defineComponent({
  setup() {
    const authData = reactive({
      email: '',
      pass: '',
    });
    const login = async () => {
      const res = await getUserByAuth(authData);
      if(res) {
        notification.success({title: '成功', message: 'ログインしました'})
        document.cookie = `session=${res.session}`
        setTimeout(()=>{
          location.href = '/'
        }, 3000)
      } else {
        notification.error({title: 'エラー', message: 'ログインに失敗しました'})
      }
    };
    return {
      authData,
      login,
    };
  },
});
</script>

<style lang="scss" module>
#authParent {
  margin: 0 auto;
  width: 70%;
  text-align: center;
  padding: 20px;
}

@media screen and (max-width: 700px) {
  #authParent {
    width: 100%;
  }
}

#authParent + a {
  margin: 10px;
}
</style>
