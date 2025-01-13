<template>
  <div id="globalHeader">
    <a-row :wrap="false">
      <a-col flex="200px">
        <RouterLink to="/">
          <div class="title-bar">
            <img class="logo" src="../assets/logo.png" alt="logo" />
            <div class="title">亿云图库</div>
          </div>
        </RouterLink>
      </a-col>
      <a-col flex="auto">
        <a-menu
          v-model:selectedKeys="current"
          mode="horizontal"
          :items="items"
          @click="doMenuClick"
        />
      </a-col>
      <a-col flex="120px">
        <div class="user-login-status">
          <a-button type="primary" href="/user/login">登录</a-button>
        </div>
      </a-col>
    </a-row>
  </div>
</template>

<script setup lang="ts">
// key 为要跳转的url路径
import { h, ref } from 'vue'
import { MailOutlined, AppstoreOutlined, HomeOutlined } from '@ant-design/icons-vue'
import { MenuProps } from 'ant-design-vue'
import { useRouter } from 'vue-router'

const router = useRouter()
const current = ref<string[]>([])
const items = ref<MenuProps['items']>([
  {
    key: '/',
    icon: () => h(HomeOutlined),
    label: '主页',
    title: '主页',
  },
  {
    key: '/about',
    label: '关于',
    title: '关于',
  },
  {
    key: 'others',
    label: h('a', { href: 'https://www.baidu.com', target: '_blank' }, 'others'),
    title: 'others',
  },
])

// 路由跳转事件 点击实现路由跳转
// 实现同步高亮
// 原理： 点击菜单时v-model绑定current变量 实现高亮
//       刷新页面时，获取当前页面的url，然后修改current变量的值， 实现同步
const doMenuClick = ({ key }: { key: string }) => {
  router.push({
    path: key,
  })
}

router.afterEach((to, from, next) => {
  current.value = [to.path]
})
</script>
<style scoped>
.title-bar {
  display: flex;
  align-items: center;
}

.title {
  color: black;
  font-size: 18px;
  margin-left: 16px;
}

.logo {
  height: 48px;
}
</style>
