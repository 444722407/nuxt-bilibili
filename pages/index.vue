<template>
  <!-- 公共头部 -->
  <AppHeader />
  <!-- 频道模块 -->
  <van-tabs>
    <van-tab v-for="item in channelList" :key="item.id" :title="item.name" />
  </van-tabs>
  <!-- 视频列表 -->
  <van-list
      v-model:loading="loading"
      :finished="finished"
      finished-text="没有更多了"
      @load="onLoad"
    >
    <div class="video-list">
      <AppVideo v-for="item in list" :key="item.bvid" :item="item"/>
    </div>
</van-list>
</template>

<style lang="scss" scoped>
  .video-list {
    display: flex;
    flex-wrap: wrap;
    padding: 10px 5px;
  }
</style>

<script setup lang="ts">
  import type {VideoItem} from '@/types/video'
  const { data: channelList } = await useFetch('/api/channel');
  const { data:videoList } = await useFetch("/api/video/video");
  useHead({
    meta:[{
      name:"referrer",
      content:"no-referrer"
    }]
  })


    const list = ref<VideoItem[]>([]);
    const loading = ref(false);
    const finished = ref(false);

    let page = 1;
    let pageSize = 20;

    const onLoad = () => {
      if(videoList.value?.length === list.value?.length){
        finished.value = true;
        return;
      }
   
     
      const data =  videoList.value?.slice((page-1)*pageSize, page* pageSize) as VideoItem[];
      list.value.push(...data)
      loading.value = false;
      page++;
    };

  onLoad()
</script>