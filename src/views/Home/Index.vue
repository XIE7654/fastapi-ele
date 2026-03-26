<template>
  <div>
    <el-card shadow="never">
      <el-skeleton :loading="loading" animated>
        <el-row :gutter="16" justify="space-between">
          <el-col :xl="12" :lg="12" :md="12" :sm="24" :xs="24">
            <div class="flex items-center">
              <el-avatar :src="avatar" :size="70" class="mr-16px">
                <img src="@/assets/imgs/avatar.gif" alt="" />
              </el-avatar>
              <div>
                <div class="text-20px">
                  {{ t('workplace.welcome') }} {{ username }} {{ t('workplace.happyDay') }}
                </div>
                <div class="mt-10px text-14px text-gray-500">
                  {{ t('workplace.toady') }}，20℃ - 32℃！
                </div>
              </div>
            </div>
          </el-col>
        </el-row>
      </el-skeleton>
    </el-card>
  </div>

  <el-row class="mt-8px" :gutter="8" justify="space-between">
    <el-col :xl="24" :lg="24" :md="24" :sm="24" :xs="24" class="mb-8px">
      <el-card shadow="never">
        <template #header>
          <div class="h-3 flex justify-between">
            <span>项目地址</span>
          </div>
        </template>
        <el-skeleton :loading="loading" animated>
          <el-row>
            <el-col
              v-for="(item, index) in projects"
              :key="`card-${index}`"
              :xl="6"
              :lg="6"
              :md="12"
              :sm="24"
              :xs="24"
            >
              <el-card
                shadow="hover"
                class="mr-5px mt-5px cursor-pointer"
                @click="handleProjectClick(item.url)"
              >
                <div class="flex items-center">
                  <Icon
                    :icon="item.icon"
                    :size="25"
                    class="mr-8px"
                    :style="{ color: item.color }"
                  />
                  <span class="text-16px">{{ item.name }}</span>
                </div>
                <div class="mt-12px text-12px text-gray-400">{{ item.description }}</div>
              </el-card>
            </el-col>
          </el-row>
        </el-skeleton>
      </el-card>
    </el-col>
  </el-row>
</template>
<script lang="ts" setup>
import { useUserStore } from '@/store/modules/user'
import type { Project } from './types'

defineOptions({ name: 'Index' })

const { t } = useI18n()
const userStore = useUserStore()
const loading = ref(true)
const avatar = userStore.getUser.avatar
const username = userStore.getUser.nickname

// 获取项目数
let projects = reactive<Project[]>([])
const getProject = async () => {
  const data: Project[] = [
    {
      name: 'FastAPI Admin',
      icon: 'simple-icons:fastapi',
      description: 'FastAPI 后端管理系统',
      url: 'https://github.com/XIE7654/fastapi-admin',
      color: '#009688'
    },
    {
      name: 'FastAPI Vben',
      icon: 'devicon:antdesign',
      description: 'Vben Admin 前端（Ant Design）',
      url: 'https://github.com/XIE7654/fastapi-vben',
      color: '#e18525'
    },
    {
      name: 'FastAPI Element',
      icon: 'ep:element-plus',
      description: 'Element Plus 前端',
      url: 'https://github.com/XIE7654/fastapi-ele',
      color: '#409EFF'
    },
    {
      name: 'Django Vue3 Admin',
      icon: 'simple-icons:django',
      description: 'Django + Vue3 管理系统',
      url: 'https://github.com/XIE7654/django-vue3-admin',
      color: '#092E20'
    }
  ]
  projects = Object.assign(projects, data)
}

const getAllApi = async () => {
  await Promise.all([getProject()])
  loading.value = false
}

const handleProjectClick = (url: string) => {
  window.open(url, '_blank')
}

getAllApi()
</script>