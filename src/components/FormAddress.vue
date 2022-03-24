<template>
  <form-item title="Адрес">
    <my-input class="address__item"
              label-text="Индекс"
              @input="setIndex"
              :v="$v.index"
              input-type="text"
              placeholder="Индекс"
              :focus="true"
              id="address-index"/>

    <my-input class="address__item"
              label-text="Страна"
              @input="setAddressCountry"
              :v="$v.addressCountry"
              input-type="text"
              placeholder="Страна"
              id="address-country"/>

    <my-input class="address__item"
              label-text="Область"
              @input="setArea"
              :v="$v.area"
              input-type="text"
              placeholder="Область"
              id="address-area"/>

    <my-input class="address__item"
              label-text="Город *"
              @input="setAddressCity"
              :v="$v.addressCity"
              input-type="text"
              placeholder="Город"
              id="address-city"/>

    <my-input class="address__item"
              label-text="Улица"
              @input="setStreet"
              :v="$v.street"
              input-type="text"
              placeholder="Улица"
              id="address-street"/>

    <my-input class="address__item"
              label-text="Дом"
              @input="setHouse"
              :v="$v.house"
              input-type="text"
              placeholder="Дом"
              id="address-house"/>
  </form-item>
</template>

<script>
import MyInput from "@/components/UI/MyInput";
import FormItem from "@/components/FormItem";
import MySelect from "@/components/UI/MySelect";
import {numeric, required} from "vuelidate/lib/validators";

export default {
  name: "FormAddress",
  components: {FormItem, MyInput, MySelect},

  data() {
    return {
      index: '',
      addressCountry: '',
      area: '',
      addressCity: '',
      street: '',
      house: '',
    }
  },

  validations: {
    index: {numeric},
    addressCountry: {},
    area: {},
    addressCity: {required},
    street: {},
    house: {},
  },

  methods: {
    setIndex(value) {
      this.index = value;
      this.$v.index.$touch();
    },
    setAddressCountry(value) {
      this.addressCountry = value;
      this.$v.addressCountry.$touch();
    },
    setArea(value) {
      this.area = value;
      this.$v.area.$touch();
    },
    setAddressCity(value) {
      this.addressCity = value;
      this.$v.addressCity.$touch();
    },
    setStreet(value) {
      this.street = value;
      this.$v.street.$touch();
    },
    setHouse(value) {
      this.house = value;
      this.$v.house.$touch();
    },
  }
}
</script>
<style scoped lang="scss">
.address__item {
  width: 100%;
  margin-bottom: 30px;
  @media (min-width: 640px) {
    width: 45%;
  }
}
</style>