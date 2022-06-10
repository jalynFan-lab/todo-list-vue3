<template>
  <button @click="show = !show">Change Button</button>
  <transition name="fade">
    <div v-if="show">transtion show</div>
  </transition>
</template>

<script>
export default {
  name: "TestTranstion",
  data() {
    return {
      show: true,
    };
  },
};
</script>

<style>
/*动画效果 transition*/
.fade-enter-active,
.fade-leave-active {
  transition: opacity 500ms ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
