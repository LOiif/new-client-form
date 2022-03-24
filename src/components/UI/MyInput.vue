<template>
  <div class="container" :class="{'container_error': v.$error}">
    <label class="label" :for="id">{{ labelText }}</label>
    <input ref="input"
           class="input"
           :type="inputType || 'text'"
           :placeholder="placeholder"
           @input="input"
           :id="id"/>
    <div class="error" v-if="v.required != undefined && !v.required && v.$error">Поле обязательно для заполения</div>

    <div class="error" v-if="v.startAtSeven != undefined && !v.startAtSeven && v.$error">
      Номер должен начинаться с 7
    </div>

    <div class="error" v-else-if="v.numeric != undefined && !v.numeric && v.$error">
      Пожалуйста, используйте цифры для заполнение этого поля
    </div>

    <div class="error" v-else-if="v.date != undefined && !v.date && v.$error">
      Пожалуйста, введите дату корректно, в формате ДД.ММ.ГГГГ
    </div>

    <div class="error" v-else-if="v.minLength != undefined && !v.minLength && v.$error">
      Поле должно содержать хотя бы {{ v.$params.minLength.min }} символов
    </div>

    <div class="error" v-else-if="v.maxLength != undefined && !v.maxLength && v.$error">
      Поле должно содержать не больше {{ v.$params.maxLength.max }} символов
    </div>

  </div>
</template>

<script>
export default {
  name: "MyInput",

  props: {
    inputType: {
      type: String,
      require: false
    },
    placeholder: {
      type: String,
      require: true
    },
    id: {
      type: String,
      require: true
    },
    labelText: {
      type: String,
      require: true
    },
    focus: {
      type: Boolean,
      require: false,
      default: false
    },
    v: {
      type: Object,
      require: true
    },
  },

  methods: {
    input(evt) {
      this.$emit('input', evt.target.value);
    }
  },

  mounted() {
    if (this.focus) {
      setTimeout(() => {
        this.$refs.input.focus();
      }, 500)
    }
  }
}
</script>

<style scoped lang="scss">
.input {
  width: 100%;
  font-family: Comfortaa, Montserrat, Avenir, Helvetica, Arial, sans-serif;;
  background-color: transparent;
  border: 1.5px solid rgba(150, 150, 150, 0.7);
  border-radius: 6px;
  padding: 12px 15px 11px 20px;
  margin-top: 15px;
  transition: all linear 0.17s;

  color: rgba(44, 62, 80, 1);

  &::placeholder {
    color: rgba(150, 150, 150, 1);
  }

  &:focus {
    outline-color: #5047b9;
  }
}

.label {
  font-size: 1.1rem;
  padding-left: 10px;
}

.container {
  width: 100%;

  @media (min-width: 640px) {
    width: 45%;
  }

  &_error {

    & .input {
      color: firebrick;
      border-color: firebrick;

      &::placeholder {
        color: rgba(150, 150, 150, 1);
      }

      &:focus {
        outline-color: firebrick;
      }
    }

    & .label {
      color: firebrick;
      padding-left: 10px;
    }
  }
}

.error {
  font-size: 14px;
  color: firebrick;
  margin-top: 5px;
  margin-left: 10px;
}
</style>