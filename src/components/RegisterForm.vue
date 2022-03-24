<template>
  <form @submit.prevent="submit" class="form">
    <div class="form__wrapper">
      <h2 class="form__title">Регистрация</h2>
      <div class="form__slider slider">

        <div class="slider__wrapper">
          <transition-group :name="animationName" ref="sliders" tag="div">
            <form-main-info key="1"
                            class="slider__item slider__item_active"
                            v-show="activeSlideNum === 1"
                            data-slide-num="1">
            </form-main-info>
            <form-address key="2"
                          class="slider__item"
                          v-show="activeSlideNum === 2"
                          data-slide-num="2">
            </form-address>
            <form-passport key="3"
                           class="slider__item"
                           v-show="activeSlideNum === 3"
                           data-slide-num="3">
            </form-passport>
          </transition-group>
        </div>

        <div class="slider__buttons">
          <my-button class="slider__prev" v-if="activeSlideNum > 1" @click="prevSlide">Назад</my-button>
          <my-button class="slider__next" v-if="activeSlideNum < slidersLength" @click="nextSlide">Далее</my-button>
          <my-button class="form__submit" type="submit" v-show="activeSlideNum === slidersLength">Зарегистрироваться
          </my-button>
        </div>

        <ul class="slider__indicators-list">
          <li class="slider__indicator" :class="{'slider__indicator_active': activeSlideNum === 1}"></li>
          <li class="slider__indicator" :class="{'slider__indicator_active': activeSlideNum === 2}"></li>
          <li class="slider__indicator" :class="{'slider__indicator_active': activeSlideNum === 3}"></li>
        </ul>
      </div>
    </div>

    <div class="modal" v-if="isShowModal">
      <div class="modal__content">
        <p class="modal__text">Регистрация прошла успешно!</p>
        <my-button type="button" @click="isShowModal = false" class="modal__button button">Ок</my-button>
      </div>
    </div>
  </form>
</template>

<script>
import MyInput from "@/components/UI/MyInput";
import MySelect from "@/components/UI/MySelect";
import FormMainInfo from "@/components/FormMainInfo";
import FormAddress from "@/components/FormAddress";
import MyButton from "@/components/UI/MyButton";
import FormPassport from "@/components/FormPassport";

export default {
  name: "RegisterForm",
  components: {FormPassport, FormAddress, FormMainInfo, MySelect, MyInput, MyButton},

  data() {
    return {
      activeSlideNum: 1,
      animationName: 'slide-next',
      slidersLength: 0,
      sliders: [],
      isShowModal: false
    }
  },
  methods: {
    nextSlide(evt) {

      const currentSlide = this.sliders.find(el => +el.elm.dataset.slideNum === this.activeSlideNum);

      currentSlide.componentInstance.$v.$touch();

      console.log(currentSlide)
      if (this.activeSlideNum < this.slidersLength && !currentSlide.componentInstance.$v.$invalid) {
        this.activeSlideNum++;
        this.animationName = 'slide-next';
      }
    },
    prevSlide(evt) {
      if (this.activeSlideNum > 1) {
        this.activeSlideNum--;
        this.animationName = 'slide-prev';
      }
    },

    submit(evt) {
      const isValid = this.sliders.every((slide) => !slide.componentInstance.$v.$invalid);
      this.sliders.forEach((slide) => slide.componentInstance.$v.$touch());
      if (isValid) {
        this.isShowModal = true;
      }
    }
  },
  mounted() {
    this.sliders = Array.from(this.$refs.sliders.children);
  },

  watch: {
    sliders() {
      this.slidersLength = this.sliders.length;
    }
  }
}
</script>

<style scoped lang="scss">

.form {
  box-shadow: 2px 2px 10px 1px rgba(70, 70, 70, 0.1);
  padding: 40px 20px 40px 20px;
  background-color: rgba(250, 250, 250, 0.9);
  margin: 0 auto;
  border-radius: 0;
  color: #5047b9;

  @media (min-width: 640px) {
    padding: 40px 60px 60px 60px;
    border-radius: 30px;
  }

  @media (min-width: 1240px) {
    width: 1200px;
  }
}
.form__title {
  text-align: center;
  font-size: 2rem;
  margin-bottom: 20px;

  @media (min-width: 640px) {
    font-size: 2.6rem;
  }
}

.form__slider {
  padding-top: 20px;
  padding-bottom: 20px;
}

.slider__item {
  border-radius: 1px;
  position: static;
  border: 1px none #5047b9;
  border-top-style: solid;

  @media (min-width: 640px) {
    border: none;
  }
}

.slider__indicators-list {
  margin: 0 auto;
  padding-top: 20px;
  display: flex;
  justify-content: center;
  list-style: none;
}

.slider__indicator {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background-color: rgba(250, 250, 250, 0.9);
  border: 1px solid #5047b9;
  transition: all ease-in-out 0.27s;

  &:not(:last-of-type) {
    margin-right: 15px;
  }

  &_active {
    transform: scale(1.4);
    background-color: #5047b9;
  }
}

.slider__buttons {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding-top: 20px;

  @media(min-width: 340px) {
    flex-direction: row;
  }
}

.slider__next {
  width: 100%;
  margin-top: 20px;

  @media(min-width: 340px) {
    width: auto;
    margin-left: auto;
    margin-top: 0;
  }
}

.form__submit {
  width: 100%;
  margin-top: 20px;
  background-color: #5047b9;
  color: rgba(250, 250, 250, 0.9);

  &:hover {
    opacity: 0.8;
  }

  @media(min-width: 340px) {
    width: auto;
    margin-left: auto;
    margin-top: 0;
  }
}

.modal {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-color: rgba(20, 20, 20, 0.5);
}

.modal__content {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-color: rgba(250, 250, 250, 0.9);
  border-radius: 20px;
  padding: 40px 20px;

  @media (min-width: 640px) {
    padding: 60px 100px;
  }
  @media (min-width: 1240px) {
    padding: 80px 120px;
  }
}

.modal__button {
  margin-top: 40px;
}

.modal__text {
  font-size: 1.4rem;

  @media (min-width: 640px) {
    font-size: 1.6rem;
  }
  @media (min-width: 1240px) {
    font-size: 2rem;
  }
}

.slide-next-enter-active {
  transition: all .4s ease-in-out;
}

.slide-prev-enter-active {
  transition: all .4s ease-in-out;
  position: absolute;
}

.slide-prev-leave-active {
  transition: all .4s ease-in-out;
}

.slide-next-leave-active {
  transition: all .4s ease-in-out;
  position: absolute;
}

.slide-next-enter {
  transform: translateX(500px);
  opacity: 0;
}

.slide-next-leave-to {
  transform: translateX(-500px);
  opacity: 0;
}

.slide-prev-enter {
  transform: translateX(-500px);
  opacity: 0;
}

.slide-prev-leave-to {
  transform: translateX(500px);
  opacity: 0;
}

</style>