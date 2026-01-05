<script lang="ts" setup>
import type { WorkbenchQuickNavItem } from '@vben/common-ui';

import { useRouter } from 'vue-router';

import { WorkbenchHeader, WorkbenchQuickNav } from '@vben/common-ui';
import { preferences } from '@vben/preferences';
import { useUserStore } from '@vben/stores';
import { openWindow } from '@vben/utils';

const userStore = useUserStore();
const router = useRouter();

// 快捷导航数据
const quickNavItems: WorkbenchQuickNavItem[] = [
  {
    color: '#1fdaca',
    icon: 'ion:home-outline',
    title: '首页',
    url: '/',
  },
  {
    color: '#3fb27f',
    icon: 'ion:person-outline',
    title: '个人中心',
    url: '/profile',
  },
  {
    color: '#3385ff',
    icon: 'ion:search-outline',
    title: '百度',
    url: 'https://www.baidu.com',
  },
  {
    color: '#00d8ff',
    icon: 'ion:chatbubble-outline',
    title: '豆包',
    url: 'https://www.doubao.com',
  },
  {
    color: '#0084ff',
    icon: 'ion:help-circle-outline',
    title: '知乎',
    url: 'https://www.zhihu.com',
  },
  {
    color: '#12b7f5',
    icon: 'ion:mail-outline',
    title: 'QQ邮箱',
    url: 'https://mail.qq.com',
  },
];

// 导航处理方法
function navTo(nav: WorkbenchQuickNavItem) {
  if (nav.url?.startsWith('http')) {
    openWindow(nav.url);
    return;
  }
  if (nav.url?.startsWith('/')) {
    router.push(nav.url).catch((error) => {
      console.error('Navigation failed:', error);
    });
  } else {
    console.warn(`Unknown URL for navigation item: ${nav.title} -> ${nav.url}`);
  }
}
</script>

<template>
  <div class="p-5">
    <!-- 欢迎卡片 -->
    <div class="welcome-header">
      <WorkbenchHeader
        :avatar="userStore.userInfo?.avatar || preferences.app.defaultAvatar"
      >
        <template #title>
          欢迎回来, {{ userStore.userInfo?.realName || '用户' }}！
        </template>
        <template #description> 祝您工作愉快！ </template>
      </WorkbenchHeader>
    </div>

    <!-- 快捷导航 -->
    <div class="mt-5">
      <WorkbenchQuickNav
        :items="quickNavItems"
        title="快捷导航"
        @click="navTo"
      />
    </div>
  </div>
</template>

<style scoped>
/* 隐藏欢迎卡片中的待办、项目、团队统计信息 */
.welcome-header :deep(.flex.justify-end) {
  display: none;
}
</style>
