<template>
  <fieldset class="form-item">
    <legend class="form-item__title">{{ title }}</legend>
    <div class="form-item__wrapper">
      <slot></slot>
    </div>
  </fieldset>
</template>

<script>
export default {
  name: "FormItem",

  props: {
    title: {
      type: String,
      required: true
    }
  }
}
</script>

<style scoped lang="scss">
.form-item__title {
  font-size: 1.4rem;
  text-align: center;

  @media (min-width: 640px) {
    font-size: 1.9rem;
    text-align: left;
  }
}

.form-item__wrapper {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  padding-top: 30px;
  padding-bottom: 20px;
}
</style>