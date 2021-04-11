<template>
  <div class="nav">
    <h3 class="brand">
      Todo Test App
    </h3>
  </div>
</template>

<script>
export default {}
</script>

<style>
.nav {
  position: fixed;
  z-index: 2;
  height: 55px;
  width: 100%;
  display: flex;
  justify-content: stretch;
  align-items: center;
  padding: 0 10px;
  background-color: #f5f5f5;
  top: 0;
  left: 0;
}
.brand {
  margin-left: 20px;
  margin-bottom: 0;
}
.nav-right {
  position: absolute;
  display: inline-block;
  left: 75%;
  transform: translateX(-50%);
}
.large-screen-nav {
  display: static;
}
.small-screen-nav {
  display: none;
  color: #dedede;
  font-size: 20px;
  line-height: 23px;
  padding-right: 10px;
}
</style>
