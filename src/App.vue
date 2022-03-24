<template>
  <div id="app">
    <register-form/>
  </div>
</template>

<script>

import RegisterForm from "@/components/RegisterForm";

export default {
  name: 'App',
  components: {RegisterForm}
}
</script>

<style lang="scss">

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  font-family: Montserrat, Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  background: rgb(101, 191, 174);
  background: linear-gradient(135deg, rgba(101, 191, 174, 1) 0%, rgba(234, 204, 255, 1) 100%);
  padding: 0;

  min-height: 100vh;

  @media (min-width: 640px) {
    padding: 80px 20px 40px 20px;
  }

  @media (min-width: 1240px) {
    padding-left: 0;
    padding-right: 0;
  }
}

.visually-hidden {
  position: absolute;
  width: 1px;
  height: 1px;
  margin: -1px;
  border: 0;
  padding: 0;
  white-space: nowrap;
  clip-path: inset(100%);
  clip: rect(0 0 0 0);
  overflow: hidden;
}
</style>
