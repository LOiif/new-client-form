<template>
  <form-item title="Документы">
    <my-select class="passport__item"
               id="passport-type"
               @input="setDocumentType"
               :v="$v.documentType"
               label-name="Тип документа *"
               :multiple="false"
               :options="passportTypeOptions">
    </my-select>

    <my-input class="passport__item"
              label-text="Серия"
              input-type="text"
              @input="setSeries"
              :v="$v.series"
              placeholder="Серия"
              id="passport-series"/>

    <my-input class="passport__item"
              label-text="Номер"
              @input="setNumber"
              :v="$v.number"
              input-type="text"
              placeholder="Номер"
              id="passport-number"/>

    <my-input class="passport__item"
              label-text="Кем выдан"
              @input="setByWhom"
              :v="$v.byWhom"
              input-type="text"
              placeholder="Кем выдан"
              id="passport-by-whom"/>

    <my-input class="passport__item"
              label-text="Дата выдачи *"
              @input="setDate"
              :v="$v.date"
              input-type="text"
              placeholder="Дата выдачи"
              id="passport-date"/>
  </form-item>
</template>

<script>
import MySelect from "@/components/UI/MySelect";
import MyInput from "@/components/UI/MyInput";
import FormItem from "@/components/FormItem";
import {numeric, required} from "vuelidate/lib/validators";
import {date} from "@/utils/Validations";

export default {
  name: "FormPassport",
  components: {MySelect, MyInput, FormItem},

  data() {
    return {
      passportTypeOptions: [
        {value: 'passport', name: 'Паспорт', selected: 'false'},
        {value: 'birthCertificate', name: 'Свидетельство о рождении', selected: 'false'},
        {value: 'waterCertificate', name: 'Вод. удостоверение', selected: 'false'},
      ],
      series: '',
      number: '',
      byWhom: '',
      date: '',
      documentType: ''
    }
  },
  validations: {
    series: {numeric},
    number: {numeric},
    byWhom: {},
    date: {required, date},
    documentType: {required},
  },

  methods: {
    setSeries(value) {
      this.series = value;
      this.$v.series.$touch();
    },

    setNumber(value) {
      this.number = value;
      this.$v.number.$touch();
    },
    setByWhom(value) {
      this.byWhom = value;
      this.$v.byWhom.$touch();
    },
    setDate(value) {
      this.date = value;
      this.$v.date.$touch();
    },
    setDocumentType(values) {
      this.documentType = values.join(', ');
      this.$v.documentType.$touch();
    },
  }
}
</script>

<style scoped lang="scss">
.passport__item {
  width: 100%;
  margin-bottom: 30px;
  @media (min-width: 640px) {
    width: 45%;
  }
}
</style>