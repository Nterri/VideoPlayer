<template>
    <div class="content">
      <router-view>

      </router-view>
    </div>
</template>

<script>
export default {
}
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: inherit;
}
@import url(https://fonts.googleapis.com/css?family=Montserrat:300,regular,500,600,italic&display=swap);
#app {
  font-style: normal;
  font-family: Montserrat;
  color: black;
  background: #fff;
  display: flex;
  min-height: 100vh;
  flex-direction: column;
}
.content {
  flex: 1 1 auto;
  display: flex;
  align-items: center;
  justify-content: center;
}
.container {
  max-width: 1120px;
  margin: 0 auto;
  padding: 0 15px;
  box-sizing: content-box;
  color: black;
  display: flex;
  align-items: center;
  justify-content: center;
  &.full {
    max-width: 100%;
    padding: 0;
    width: 100%;
    height: 100%;
  }
}
</style>