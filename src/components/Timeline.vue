
<script setup lang="ts">
import { DateTime } from "luxon";
import { ref, computed } from "vue";
import { Post, today, thisWeek, thisMonth } from "../posts";

const periods = ["Today", "This Week", "This Month"] as const;

type Period = typeof periods[number];

const selectedPeriod = ref<Period>("Today");

const selectPeriod = (period: Period) => {
  selectedPeriod.value = period;
};

const posts = computed(() => {
return [today, thisWeek, thisMonth]
  .map((post) => {
    return {
      ...post,
      created: DateTime.fromISO(post.created),
    }
  })
  .filter((post) => {
    if(selectedPeriod.value === "Today") {
      return post.created >= DateTime.now().minus({ day: 1 });
    }

    if (selectedPeriod.value === "This Week") {
      return post.created >= DateTime.now().minus({ week: 1 });
    }

    return post
  })
});
</script>


<template>
  <div>
    <nav class="is-primary panel">
      <span class="panel-tabs">
        <a
          v-for="period in periods"
          :key="period"
          @click="selectPeriod(period)"
          :class="{ 'is-active': selectedPeriod === period }"
          href="#"
          >{{ period }}</a
        >
      </span>
    </nav>
    <a v-for="post of posts" :key="post.id" class="panel-block">
      <a>{{ post.title }}</a>
      <div>{{ post.created.toFormat("d MMM") }}</div>
    </a>
  </div>
</template>


<style>
</style>