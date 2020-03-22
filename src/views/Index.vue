<template lang="pug">
  InfiniteScroll(
    :height="500"
    :itemHeight="150"
    :data="repeatHundredTimesRepoData"
  )
    template(v-slot:item="{ item }")
      .data-item
        span title: {{ item.name }}
        span description: {{ item.description }}
        span url:
          |
          |
          a(
            target="_blank"
            :href="item.html_url"
          ) {{ item.html_url }}
</template>

<script lang="ts">
import Vue from 'vue';
import Component from 'vue-class-component';

import axios from 'axios';

import InfiniteScroll from '@/components/InfiniteScroll.vue';

const PUBLIC_REPO_URL = 'https://api.github.com/users/Wei12345/repos';

@Component({
  components: {
    InfiniteScroll
  }
})
export default class Index extends Vue {
  repoData = [];

  async created() {
    const { data } = await axios.get(PUBLIC_REPO_URL);
    this.repoData = data;
  }

  get repeatHundredTimesRepoData() {
    const data: any[] = [];
    data.length = 100;
    return Array.from(data)
      .fill(this.repoData)
      .flat();
  }
}
</script>

<style lang="scss" scoped>
.data-item {
  width: 100%;
  height: 100%;
  padding: 10px 0px;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  align-items: center;
  border: 1px solid black;
  box-sizing: border-box;
}
</style>
