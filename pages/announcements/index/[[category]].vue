<template>
  <base-scroll-pagination @paginate="fetchMore()">
    <div class="announcement-list">
      <announcement-card
          class="announcement-list__item"
          v-for="announcement in searchStore.getList"
          :info="announcement"
      />
    </div>
  </base-scroll-pagination>

  <base-loader v-if="isLoading" center-horizontal/>
</template>

<script setup>
import {useRoute} from "nuxt/app";
import BaseScrollPagination from "../../../components/base/BaseScrollPagination";
import AnnouncementCard from "../../../components/common/miniCards/announcementCard";
import {useSearchAnnouncementStore} from "../../../store/search";
import {computed, ref, watch} from "vue";
import BaseLoader from "../../../components/base/BaseLoader";

const route = useRoute();

const searchStore = useSearchAnnouncementStore();
const activeCategory = computed(() => route.params.category ? categoryStore.getList?.find(c => c.code === route.params.category) : null)

const searchParams = computed(() => ({
  category: route.params.category,
}))

const isLoading = ref(true);

watch(() => searchParams, async () => {
  isLoading.value = true;
  await searchStore.fetchListInit(searchParams.value);
  isLoading.value = false;
}, {
  immediate: true,
  deep: true
})

const fetchMore = async () => {
  if (isLoading.value) return;
  isLoading.value = true;
  await searchStore.fetchListMore();
  isLoading.value = false;
}
</script>

<style lang="scss" scoped>
.announcement-list {
  display: grid;
  grid-gap: 1rem;
  grid-template-columns: repeat(auto-fill, minmax(170px, 1fr));
  margin: .5rem 0;

}
</style>