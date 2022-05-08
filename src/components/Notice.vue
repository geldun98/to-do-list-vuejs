<template>
  <transition name="fade">
    <div class="notice">
      <h1>Notice</h1>
      <p>Delete task success</p>
    </div>
  </transition>
</template>

<script>
export default {
  name: "notice-comp",
};
</script>

<style lang="scss" scoped>
.notice {
  display: block;
  width: 250px;
  background: #2ecc71;
  color: white;
  padding: 10px 20px;
  position: fixed;
  top: 0;
  right: 20px;
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 1s linear;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
