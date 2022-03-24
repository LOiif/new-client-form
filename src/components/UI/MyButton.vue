<template>
  <button class="button" type="button" @click="$emit('click', $event)">
    <slot></slot>
  </button>
</template>

<script>
export default {
  name: "MyButton",
}
</script>

<style scoped lang="scss">
.button {
  background-color: transparent;
  padding: 10px 30px;
  border: 1px solid #5047b9;
  border-radius: 6px;
  color: rgba(44, 62, 80, 1);;
  transition: all ease-in-out 0.17s;

  &:hover {
    background-color: #5047b9;
    color: rgba(250, 250, 250, 0.9);
  }

  &:active {
    opacity: 0.5;
  }

  @media (min-width: 640px) {
    padding: 10px 38px;
  }
}

</style>