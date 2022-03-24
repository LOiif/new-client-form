<template>
  <div class="select" :class="{'select_error': v.$error}">
    <label class="select__label" :for="id">{{ labelName }}</label>
    <div class="select__wrapper">
      <input class="select__input"
             ref="input"
             :value="inputText"
             :id="id"
             @click="select"
             @keydown="keyDown"
             autocomplete="off"
             placeholder="Выберите элемент из списка:"/>

      <span class="select__indicator" @click="select" ref="indicator"></span>

      <div class="select__content" ref="optionsEl" :class="{'hidden': !showOptions}">
        <button class="select__option"
                @focusin="optionFocus"
                @blur="optionBlur"
                type="button"
                :class="{'select__option_multiple': multiple, 'select__option_no-multiply': !multiple}"
                v-for="(opt, i) in options"
                :key="opt.value + i"
                :data-selected="selectedOptions[opt.value]"
                @mouseover="optionMouseOver"
                @click.prevent="optionClick"
                :data-value="opt.value">{{ opt.name }}
        </button>
      </div>
    </div>
    <div class="error" v-if="v.required != undefined && !v.required && v.$error">Поле обязательно для заполения</div>
  </div>
</template>

<script>
export default {
  name: "MySelect",

  data() {
    return {
      showOptions: false,
      selectedOptions: Array.from(this.options.values()).reduce((acc, el) => {
        return Object.assign(acc, {[el.value]: el.selected})
      }, {}),
      inputText: '',
      focusEl: '',
      focusCounter: 0
    }
  },
  props: {
    labelName: {
      type: String,
      required: true
    },
    id: {
      type: String,
      required: true
    },
    options: {
      type: Array,
      required: true
    },
    multiple: {
      type: Boolean,
      required: false,
      default: false
    },
    v: {
      type: Object,
      require: true
    },
  },
  methods: {
    select(evt) {
      this.$refs.input.classList.toggle('select__input_focus');
      this.$refs.indicator.classList.toggle('select__indicator_focus');

      this.showOptions = !this.showOptions;

      if (this.showOptions) {
        const options = Array.from(this.$refs.optionsEl.children);
        options.forEach(el => {
          if (this.selectedOptions[el.dataset.value] === 'true') {
            el.classList.add('select__option_active');
          }
        })
      }
    },
    optionClick(evt) {
      const optionsEls = Array.from(this.$refs.optionsEl.children);

      if (!this.multiple) {
        this.inputText = evt.target.textContent;
        optionsEls.forEach((el) => {
          el.classList.remove('select__option_active');
          this.selectedOptions[el.dataset.value] = 'false';
        })
        this.$refs.input.classList.remove('select__input_focus');
        this.$refs.indicator.classList.remove('select__indicator_focus');
        evt.target.classList.add('select__option_active');
        this.selectedOptions[evt.target.dataset.value] = 'true';
        this.showOptions = false;
      } else {
        evt.target.classList.toggle('select__option_active');
        this.inputText = '';
        this.selectedOptions[evt.target.dataset.value] = (this.selectedOptions[evt.target.dataset.value] === 'false')
            ? 'true'
            : 'false'

        const inputTextArr = [];

        for (let key in this.selectedOptions) {
          if (this.selectedOptions[key] === 'true') {
            inputTextArr.push(optionsEls.find(el => el.dataset.value === key).textContent);
          }
        }
        this.inputText = inputTextArr.join(', ');
      }

      const selectedValues = [];

      for (const key in this.selectedOptions) {
        if (this.selectedOptions[key] === 'true') {
          selectedValues.push(key);
        }
      }

      this.$emit('input', selectedValues);
    },

    optionFocus(evt) {
      this.focusCounter++;
    },

    optionBlur() {
      this.focusCounter--;

      setTimeout(() => {
        if (this.focusCounter === 0) {
          this.$refs.input.classList.remove('select__input_focus');
          this.$refs.indicator.classList.remove('select__indicator_focus');
          this.showOptions = false;
        }
      }, 0)
    },
    keyDown(evt) {
      if (evt.code === 'Enter' || evt.code === 'Space') {
        evt.preventDefault();
        this.select(evt);
      } else if (evt.code !== 'Tab') {
        evt.preventDefault();
      }
    },
    optionMouseOver(evt) {
      if (!this.multiple) {
        evt.target.classList.add('select__option_active');
        const options = Array.from(this.$refs.optionsEl.children);
        options.forEach(el => {
          if (el !== evt.target) {
            el.classList.remove('select__option_active');
          }
        })
      }
    },
    hideSelect(evt) {
      if (this) {
        if (!evt.target.closest('.select__wrapper')) {
          this.showOptions = false;
          this.$refs.input.classList.remove('select__input_focus');
          this.$refs.indicator.classList.remove('select__indicator_focus');
        }
      }
    },
  },

  mounted() {
    document.addEventListener('click', this.hideSelect);
  },
  beforeDestroy() {
    document.removeEventListener('click', this.hideSelect);
  },
}
</script>

<style scoped lang="scss">

.select__label {
  padding-left: 10px;
  font-size: 1.1rem;
}

.select__wrapper {
  position: relative;
  padding-top: 15px;
}

.select__input {
  width: 100%;
  font-family: Comfortaa, Montserrat, Avenir, Helvetica, Arial, sans-serif;;
  background-color: transparent;
  border: 1.5px solid rgba(150, 150, 150, 0.7);
  border-radius: 6px;
  padding: 12px 35px 11px 20px;
  transition: all ease-in-out 0.17s;
  color: rgba(44, 62, 80, 1);
  cursor: default;
  caret-color: transparent;

  &:focus {
    outline: none;
    border: 2px solid #5047b9;
  }

  &_focus {
    outline: none;
    border: 2px solid #5047b9;
    border-bottom-left-radius: 0;
    border-bottom-right-radius: 0;
  }
}

.select__content {
  display: flex;
  flex-direction: column;
  box-shadow: 4px 10px 15px 1px rgba(70, 70, 70, 0.15);
  border: 2px solid #5047b9;
  position: absolute;
  background-color: rgba(250, 250, 250, 0.9);
  margin-top: -3px;
  z-index: 2;
  color: rgba(44, 62, 80, 1);
  border-bottom-right-radius: 6px;
  border-bottom-left-radius: 6px;
  width: 100%;
  cursor: default;
  caret-color: transparent;
  transition: all ease-in-out 0.17s;
}

.select__option {
  text-align: left;
  font-family: inherit;
  color: rgba(44, 62, 80, 1);
  background-color: rgba(250, 250, 250, 0.9);
  border: none;
  padding: 8px 15px 8px 20px;

  &:focus {
    outline: none;
    border: 1px solid #5047b9;
  }

  &:active {
    opacity: 0.8;
  }

  &_no-multiply {
    &:hover {
      color: white;
      background-color: rgba(80, 71, 185, 0.5);
    }
  }

  &_multiple {
    &:hover {
      opacity: 0.7;
    }
  }

  &_active {
    color: white;
    background-color: rgba(80, 71, 185, 0.5);
    box-shadow: inset 0 0 1px 1px rgba(70, 70, 70, 0.2);
  }
}

.select__indicator {
  content: '';
  position: absolute;
  right: 15px;
  top: 50%;
  width: 18px;
  height: 18px;
  mask: url("@/assets/images/arrow.svg");
  background-color: rgba(150, 150, 150, 0.7);
  transition: all ease-in-out 0.17s;

  &_focus {
    background-color: #5047b9;
    transform: rotate(-180deg);
  }
}

.select {
  &_error {

    & .select__indicator {
      background-color: firebrick;
    }

    & .select__content {
      border-color: firebrick;
    }

    & .select__input {
      border-color: firebrick;

      &_focus {
        border-color: firebrick;
      }
    }

    & .select__label {
      color: firebrick;
    }
  }
}

.error {
  margin-top: 5px;
  margin-left: 10px;
  font-size: 14px;
  color: firebrick
}

.hidden {
  display: none;
}

</style>