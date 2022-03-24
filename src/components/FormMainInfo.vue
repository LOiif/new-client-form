<template>
  <form-item title="Основная информация">
    <my-input class="main-info__item"
              label-text="Фамилия *"
              @input="setLastName"
              :v="$v.lastName"
              input-type="text"
              :focus="true"
              placeholder="Фамилия"
              id="lastname"/>

    <my-input class="main-info__item"
              label-text="Имя"
              @input="setName"
              :v="$v.name"
              input-type="text"
              placeholder="Имя"
              id="name"/>

    <my-input class="main-info__item"
              label-text="Отчество"
              @input="setPatronymic"
              :v="$v.patronymic"
              input-type="text"
              placeholder="Отчество"
              id="patronymic"/>

    <my-input class="main-info__item"
              label-text="Дата рождения *"
              @input="setBirthday"
              :v="$v.birthday"
              input-type="text"
              placeholder="ДД.ММ.ГГГГ"
              id="date-of-birth"/>

    <my-input class="main-info__item"
              label-text="Номер телефона *"
              @input="setTelephone"
              :v="$v.telephone"
              input-type="tel"
              placeholder="79999999"
              id="telephone-number"/>

    <div class="main-info__item">
      <h4 class="main-info__radio-title">Пол</h4>
      <div class="main-info__radio-wrapper">
        <label class="main-info__radio-label">
          <input class="main-info__radio visually-hidden" type="radio" name="gender" value="male" checked/>
          Мужской
          <span class="main-info__radio-indicator"></span>
        </label>

        <label class="main-info__radio-label">
          <input class="main-info__radio visually-hidden" type="radio" name="gender" value="male"/>
          Женский
          <span class="main-info__radio-indicator"></span>
        </label>
      </div>
    </div>

    <my-select class="main-info__item"
               id="client-group"
               label-name="Группа клиентов *"
               @input="setClients"
               :v="$v.clients" :multiple="true"
               :options="clientGroupOptions">
    </my-select>

    <my-select class="main-info__item"
               id="treating-doctor"
               label-name="Лечащий врач"
               @input="setDoctor"
               :v="$v.doctor"
               :multiple="false"
               :options="treatingDoctorOptions">
    </my-select>

    <div class="main-info__item">
      <div class="main-info__checkbox-wrapper">
        <label class="main-info__checkbox-label">
          <input class="main-info__checkbox visually-hidden" type="checkbox" name="dont-sent-sms" />
          <span class="main-info__checkbox-indicator"></span>
          Не отправлять СМС
        </label>
      </div>
    </div>
  </form-item>
</template>

<script>
import MySelect from "@/components/UI/MySelect";
import MyInput from "@/components/UI/MyInput";
import formItem from "@/components/FormItem";
import {minLength, numeric, required, maxLength} from "vuelidate/lib/validators";
import {date, startAtSeven} from "@/utils/Validations";

export default {
  name: "FormMainInfo",
  components: {MySelect, MyInput, formItem},

  data() {
    return {
      clientGroupOptions: [
        {value: 'vip', name: 'Vip', selected: 'false'},
        {value: 'problem', name: 'Проблемные', selected: 'false'},
        {value: 'oms', name: 'ОМС', selected: 'false'},
      ],
      treatingDoctorOptions: [
        {value: 'ivanov', name: 'Иванов', selected: 'false'},
        {value: 'zaharov', name: 'Захаров', selected: 'false'},
        {value: 'chernishova', name: 'Чернышева', selected: 'false'},
      ],

      lastName: '',
      name: '',
      patronymic: '',
      birthday: '',
      telephone: '',
      clients: '',
      doctor: ''
    }
  },

  validations: {
    lastName: {required,},
    name: {},
    patronymic: {},
    birthday: {
      required,
      minLength: minLength(8),
      maxLength: maxLength(10),
      date
    },
    telephone: {
      required,
      minLength: minLength(11),
      maxLength: maxLength(11),
      startAtSeven,
      numeric
    },
    clients: {required},
    doctor: {},
  },

  methods: {
    setLastName(value) {
      this.lastName = value;
      this.$v.lastName.$touch();
    },
    setName(value) {
      this.name = value;
      this.$v.name.$touch();
    },
    setPatronymic(value) {
      this.patronymic = value;
      this.$v.patronymic.$touch();
    },
    setBirthday(value) {
      this.birthday = value;
      this.$v.birthday.$touch();
    },
    setTelephone(value) {
      this.telephone = value;
      this.$v.telephone.$touch();
    },
    setClients(values) {
      this.clients = values.join(', ');
      this.$v.clients.$touch();
    },
    setDoctor(values) {
      this.doctor = values.join(', ');
      this.$v.doctor.$touch();
    },
  }
}
</script>

<style scoped lang="scss">
.main-info__item {
  width: 100%;
  margin-bottom: 30px;
  @media (min-width: 640px) {
    width: 45%;
  }
}

.main-info__radio-title {
  font-weight: 400;
  padding-left: 10px;
  font-size: 1.1rem;
}

.main-info__radio-wrapper {
  display: flex;
  align-items: center;
  padding-top: 12px;

  @media (min-width: 640px) {
    padding-top: 24px;
  }
}

.main-info__radio-label {
  display: flex;
  align-items: center;
  flex-direction: row;
  position: relative;
  padding-left: 24px;

  &:not(:last-of-type) {
    margin-right: 10%;
  }

  &:hover {
    opacity: 0.8;
  }

  &:active {
    opacity: 0.5;
  }

  & input:checked ~ .main-info__radio-indicator {
    border: 1px solid #5047b9;
  }

  & input:checked ~ .main-info__radio-indicator::after {
    display: block;
  }
}

.main-info__radio-indicator {
  content: '';
  position: absolute;
  width: 16px;
  height: 16px;
  top: 50%;
  left: 0;
  margin-top: -8px;
  border: 1px solid rgba(150, 150, 150, 1);
  border-radius: 50%;

  &::after {
    content: '';
    position: absolute;
    display: none;
    width: 6px;
    height: 6px;
    top: 50%;
    left: 50%;
    margin-left: -3px;
    margin-top: -3px;
    border-radius: 50%;
    background-color: #5047b9;
  }
}

.main-info__checkbox-wrapper {
  display: flex;
  align-items: center;
  padding-top: 12px;

  @media (min-width: 640px) {
    padding-top: 24px;
  }
}

.main-info__checkbox-label {
  display: flex;
  align-items: center;
  flex-direction: row;
  position: relative;
  padding-left: 30px;
  font-size: 1.1rem;

  &:not(:last-of-type) {
    margin-right: 10%;
  }

  &:hover {
    opacity: 0.8;
  }

  &:active {
    opacity: 0.5;
  }

  & input:checked ~ .main-info__checkbox-indicator {
    border-color: #5047b9;
  }

  & input:checked ~ .main-info__checkbox-indicator::after {
    display: block;
  }
}

.main-info__checkbox-indicator {
  content: '';
  position: absolute;
  width: 16px;
  height: 16px;
  top: 50%;
  left: 0;
  margin-top: -8px;
  border: 1px solid rgba(150, 150, 150, 1);
  border-radius: 2px;

  &::after {
    content: '';
    position: absolute;
    display: none;
    width: 14px;
    height: 14px;
    top: 50%;
    left: 50%;
    margin-left: -7px;
    margin-top: -7px;
    background-repeat: no-repeat;
    background-position: center;
    background-size: cover;
    background-image: url("@/assets/images/mark.svg");
  }
}
</style>