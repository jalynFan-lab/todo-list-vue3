<template>
  <li :class="{ done: done }" @click="handleClick">
    <span v-if="done">✓</span>
    <span v-else>□</span>
    <!-- {{ text }} -->
    <slot />
  </li>
</template>

<script>
export default {
  name: "TodoItem",
  props: {
    // text: {
    //   type: String,
    // },
    done: {
      type: Boolean,
    },
  },
  emits: ["toggle"],
  methods: {
    handleClick() {
      this.$emit("toggle");
    },
  },
};
</script>

<style>
.done {
  text-decoration: line-through;
}
</style>
