<template>
  <div class="index-page">
    <a-input-search
      v-model:value="searchParams.text"
      placeholder="input search text"
      enter-button="Search"
      size="large"
      @search="onSearch"
    />
  </div>
  <MyDivider />
  <a-tabs v-model:activeKey="activeKey" @change="onTabChange">
    <a-tab-pane key="post" tab="文章">
      <PostList :post-list="postList" />
    </a-tab-pane>
    <a-tab-pane key="picture" tab="图片">
      <PictureList />
    </a-tab-pane>
    <a-tab-pane key="user" tab="用户">
      <UserList :user-list="userList" />
    </a-tab-pane>
  </a-tabs>
</template>

<script setup lang="ts">
import { ref, watchEffect } from "vue";
import PostList from "@/components/PostList.vue";
import UserList from "@/components/UserList.vue";
import PictureList from "@/components/PictureList.vue";
import MyDivider from "@/components/MyDivider.vue";
import { useRoute, useRouter } from "vue-router";
import myAxios from "@/plugins/myAxios";

const postList = ref([]);

myAxios.post("post/list/page/vo", {}).then((res: any) => {
  postList.value = res.records;
});

const userList = ref([]);

myAxios.post("user/list/page/vo", {}).then((res: any) => {
  console.log(res);
  userList.value = res.records;
});

// const searchText = ref("");
const router = useRouter();
const route = useRoute();
const activeKey = route.params.category;

const initSearchParams = {
  text: "",
  pageSize: 10,
  pageNum: 1,
};

const searchParams = ref(initSearchParams);

watchEffect(() => {
  searchParams.value = {
    // 这里是用于兜底的，最起码让他有个初始值可以用
    ...initSearchParams,
    text: route.query.text,
  } as any;
});

const onSearch = (value: string) => {
  alert(value);
  router.push({
    query: searchParams.value,
  });
};

const onTabChange = (key: string) => {
  router.push({
    path: `/${key}`,
    query: searchParams.value,
  });
};
</script>
