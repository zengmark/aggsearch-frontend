<template>
  <div class="index-page">
    <a-input-search
      v-model:value="searchText"
      placeholder="input search text"
      enter-button="Search"
      size="large"
      @search="onSearch"
    />
  </div>
  <MyDivider />
  <a-tabs v-model:activeKey="activeKey">
    <a-tab-pane key="post" tab="文章">
      <PostList />
    </a-tab-pane>
    <a-tab-pane key="picture" tab="图片">
      <UserList />
    </a-tab-pane>
    <a-tab-pane key="user" tab="用户">
      <PictureList />
    </a-tab-pane>
  </a-tabs>
</template>

<script setup lang="ts">
import { ref } from "vue";
import PostList from "@/components/PostList.vue";
import UserList from "@/components/UserList.vue";
import PictureList from "@/components/PictureList.vue";
import MyDivider from "@/components/MyDivider.vue";

const searchText = ref("");
const activeKey = ref("post");

const onSearch = (value: string) => {
  alert(value);
};
</script>
