<template>
  <div class="base-scroll-pagination" :ref="el => scrollComponent = el">
    <slot/>
  </div>
</template>

<script setup>
import {onMounted, onUnmounted, ref} from "vue";

const emits = defineEmits(["paginate"])
const props = defineProps({
  loading: {
    type: Boolean,
    default: false
  }
})

const page = ref(1);

const scrollComponent = ref(null);
const handleScroll = (e) => setTimeout(() => {
  if (props.loading) return;
  let element = scrollComponent.value
  if (element.getBoundingClientRect().bottom < window.innerHeight) {
    emits("paginate", page.value++);
  }
})

onMounted(() => {
  window.addEventListener("scroll", handleScroll)
})

onUnmounted(() => {
  window.removeEventListener("scroll", handleScroll)
})
</script>

<style scoped>

</style>