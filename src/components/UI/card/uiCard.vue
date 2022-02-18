<template>
  <div class="card">
    <slot></slot>
    <div class="blur"></div>
  </div>
</template>

<script>
export default {
  name: "uiCard",
};
</script>

<style lang="scss" scoped>
.card {
  background: linear-gradient(
    143.98deg,
    rgba(255, 255, 255, 0.11) 0%,
    rgba(255, 255, 255, 0.22) 100%
  );
  backdrop-filter: blur(50px);

  border-radius: 30px;

  width: 50px;
  margin: auto;

  padding: 10px 10px;

  overflow: hidden;
}
.blur {
  border: 3px solid #02f0ff;
  box-sizing: border-box;
  box-shadow: inset 0 4px 0 rgba(0, 0, 0, 0.25);
  filter: blur(80px);
  border-radius: 30px;
}
</style>
