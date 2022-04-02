<template>
  <div id="app">
    <div class="wrap">
      <form
        action=""
        method="post"
        name="client_form"
        class="form"
        @keydown="changeEnterForm"
        @submit.prevent="checkForm"
      >
        <ul class="form_list">
          <h2>Заполните данные клиента</h2>
          <li>
            <label for="surname">Фамилия*</label>
            <input
              @blur="v$.form.surname.$touch"
              type="text"
              id="surname"
              name="surname"
              :class="{ is_invalid: v$.form.surname.$error }"
              v-model.trim="form.surname"
            />
            <p
              v-if="v$.form.surname.required.$invalid && v$.form.surname.$error"
              class="invalid"
            >
              Это обязательное поле
            </p>
          </li>
          <li>
            <label for="name">Имя*</label>
            <input
              @blur="v$.form.name.$touch"
              type="text"
              id="name"
              name="name"
              :class="{ is_invalid: v$.form.name.$error }"
              v-model.trim="form.name"
            />
            <p
              v-if="v$.form.name.required.$invalid && v$.form.name.$error"
              class="invalid"
            >
              Это обязательное поле
            </p>
          </li>
          <li>
            <label for="middlename">Отчество</label>
            <input
              type="text"
              id="middlename"
              name="middlename"
              v-model.trim="form.middlename"
            />
          </li>
          <li>
            <label for="birthday">Дата рождения*</label>
            <input
              @blur="v$.form.birthday.$touch"
              type="date"
              id="birthday"
              name="birthday"
              :class="{ is_invalid: v$.form.birthday.$error }"
              v-model="form.birthday"
            />
            <p
              v-if="
                v$.form.birthday.required.$invalid && v$.form.birthday.$error
              "
              class="invalid"
            >
              Это обязательное поле
            </p>
            <p
              v-if="
                v$.form.birthday.between.$invalid && v$.form.birthday.$error
              "
              class="invalid"
            >
              Это действительно ваша дата рождения?
            </p>
          </li>
          <li>
            <label for="tel">Номер телефона*</label>
            <input
              @blur="v$.form.tel.$touch"
              type="tel"
              id="tel"
              name="tel"
              :class="{ is_invalid: v$.form.tel.$error }"
              v-model.trim="form.tel"
            />
            <p
              v-if="v$.form.tel.required.$invalid && v$.form.tel.$error"
              class="invalid"
            >
              Это обязательное поле
            </p>
            <p
              v-if="v$.form.tel.checkLength.$invalid && v$.form.tel.$error"
              class="invalid"
            >
              Пожалуйста введите ваш номер, должно быть 11 цифр
            </p>
          </li>
          <li>
            <label for="gender">Пол</label>
            <input
              type="radio"
              id="male"
              name="gender"
              value="male"
              v-model="form.gender"
            /><label class="light" for="male">Мужской</label>
            <br />
            <input
              type="radio"
              id="female"
              name="gender"
              value="female"
              v-model="form.gender"
            /><label class="light" for="female">Женский</label>
          </li>
          <li class="wrap_group">
            <label for="client_group" class="label_group"
              >Группа клиента*</label
            >
            <select
              @blur="v$.form.group.$touch"
              class="client_group"
              name="client_group"
              id="client_group"
              multiple
              :class="{ is_invalid: v$.form.group.$error }"
              v-model="form.group"
            >
              <option disabled>
                Выберите группы к которым вы принадлежите
              </option>
              <option
                v-for="(group, index) in groups"
                :key="index"
                :value="group.value"
              >
                {{ group.label }}
              </option>
            </select>
            <p class="hint">
              Подсказка: Вы можете выбрать несколько вариантов с помощью зажатой
              клавишей Ctrl
            </p>
            <p
              v-if="v$.form.group.required.$invalid && v$.form.group.$error"
              class="invalid"
            >
              Выберите хотя бы один вариант
            </p>
          </li>
          <li>
            <label for="doctor">Лечащий врач</label>
            <select name="doctor" id="doctor" v-model="form.doctor">
              <option
                v-for="(doctor, index) in doctors"
                :value="doctor.value"
                :key="index"
              >
                {{ doctor.label }}
              </option>
            </select>
          </li>
          <li>
            <input
              type="checkbox"
              id="send_sms"
              name="send_sms"
              v-model="form.sendSms"
            />
            <label class="light" for="send_sms">Не отправлять СМС</label>
          </li>
          <h2>Адрес</h2>
          <li>
            <label for="index">Индекс</label>
            <input
              type="text"
              id="index"
              name="index"
              v-model.trim="form.index"
            />
          </li>
          <li>
            <label for="country">Страна</label>
            <input
              type="text"
              id="country"
              name="country"
              v-model.trim="form.country"
            />
          </li>
          <li>
            <label for="region">Область</label>
            <input
              type="text"
              id="region"
              name="region"
              v-model.trim="form.region"
            />
          </li>
          <li>
            <label for="city">Город*</label>
            <input
              @blur="v$.form.city.$touch"
              type="text"
              id="city"
              name="city"
              :class="{ is_invalid: v$.form.city.$error }"
              v-model.trim="form.city"
            />
            <p
              v-if="v$.form.city.required.$invalid && v$.form.city.$error"
              class="invalid"
            >
              Это обязательное поле
            </p>
          </li>
          <li>
            <label for="street">Улица</label>
            <input
              type="text"
              id="street"
              name="street"
              v-model.trim="form.street"
            />
          </li>
          <li>
            <label for="homenumber">Дом</label>
            <input
              type="text"
              id="homenumber"
              name="homenumber"
              v-model.trim="form.homenumber"
            />
          </li>
          <h2>Документ подтверждайщий личность</h2>
          <li>
            <label for="document">Тип документа*</label>
            <select name="document" id="document" v-model="form.document">
              <option disabled>Выберите тип документа</option>
              <option
                v-for="(document, index) in documents"
                :value="document.value"
                :key="index"
              >
                {{ document.label }}
              </option>
            </select>
          </li>
          <li>
            <label for="series">Серия</label>
            <input
              type="text"
              id="series"
              name="series"
              v-model.trim="form.series"
            />
          </li>
          <li>
            <label for="number">Номер</label>
            <input
              type="text"
              id="number"
              name="number"
              v-model.trim="form.number"
            />
          </li>
          <li>
            <label for="issued_by">Кем выдан</label>
            <input
              type="text"
              id="issued_by"
              name="issued_by"
              v-model.trim="form.issuedBy"
            />
          </li>
          <li>
            <label for="date_issued">Дата выдачи*</label>
            <input
              @blur="v$.form.dateIssued.$touch"
              type="date"
              id="date_issued"
              name="date_issued"
              :class="{ is_invalid: v$.form.dateIssued.$error }"
              v-model="form.dateIssued"
            />
            <p
              v-if="
                v$.form.dateIssued.required.$invalid &&
                v$.form.dateIssued.$error
              "
              class="invalid"
            >
              Это обязательное поле
            </p>
            <p
              v-if="
                v$.form.dateIssued.between.$invalid && v$.form.dateIssued.$error
              "
              class="invalid"
            >
              Введите корректную дату выдачи документа
            </p>
          </li>
          <li><button type="submit">Отправить</button></li>
        </ul>
      </form>
    </div>
  </div>
</template>

<script>
import useVuelidate from '@vuelidate/core'
import { required } from '@vuelidate/validators'

export default {
  name: 'App',
  setup() {
    return { v$: useVuelidate() }
  },
  data() {
    return {
      form: {
        surname: '',
        name: '',
        middlename: '',
        gender: '',
        birthday: '',
        tel: '+7',
        sendSms: false,
        index: '',
        country: '',
        region: '',
        city: '',
        street: '',
        homenumber: '',
        series: '',
        number: '',
        issuedBy: '',
        dateIssued: '',
        group: [],
        doctor: 'Ivanov',
        document: 'passport',
      },
      groups: [
        {
          label: 'VIP',
          value: 'VIP',
        },
        {
          label: 'Проблемные',
          value: 'problematic',
        },
        {
          label: 'ОМС',
          value: 'OMS',
        },
      ],
      doctors: [
        {
          label: 'Иванов',
          value: 'Ivanov',
        },
        {
          label: 'Захаров',
          value: 'Zaharov',
        },
        {
          label: 'Чернышева',
          value: 'Chernisheva',
        },
      ],
      documents: [
        {
          label: 'Паспорт',
          value: 'passport',
        },
        {
          label: 'Свидетельство о рождении',
          value: 'birthCertificate',
        },
        {
          label: 'Вод. удостоверение',
          value: 'driversLicense',
        },
      ],
    }
  },
  validations() {
    return {
      form: {
        surname: {
          required,
        },
        name: { required },
        birthday: {
          required,
          between: (value) => {
            if (
              Date.parse(value) > Date.parse('1900-01-01') &&
              Date.parse(value) < Date.now()
            ) {
              return true
            } else {
              return false
            }
          },
        },
        tel: {
          required,
          checkLength(value) {
            return value.length === 12
          },
        },
        group: {
          required(value) {
            return value.length > 0
          },
        },
        city: {
          required,
        },
        dateIssued: {
          required,
          between: (value) => {
            if (
              Date.parse(value) > Date.parse('1900-01-01') &&
              Date.parse(value) < Date.now()
            ) {
              return true
            } else {
              return false
            }
          },
        },
      },
    }
  },
  methods: {
    changeEnterForm(e) {
      if (e.keyCode == 13) {
        e.preventDefault()
        return false
      }
    },
    checkForm() {
      this.v$.form.$touch()
      if (this.v$.form.$error) {
        alert(
          'Данные заполненны не полностью или некорректно! Необходимые для заполнения поля отмечены красным.'
        )
      } else {
        alert('Клиент создан')
      }
    },
  },
}
</script>

<style lang="sass">
*, *:before, *:after
  -moz-box-sizing: border-box
  -webkit-box-sizing: border-box
  box-sizing: border-box

body
  margin: 0

ul
  margin: 0

li
  list-style: none
  margin-bottom: 10px

.wrap
  background-color: #f0f0f0

.form
  max-width: 800px
  margin: auto
  align-items: center
  border: 1px solid #a3a3a3
  border-top: none
  border-bottom: none
  padding: 30px 0
  background-color: #e6e6e6
  &_list
    padding: 0 40px

input[type="text"], input[type="date"],
input[type="tel"], select
  background-color: #ffffff
  border: 1px solid #e5e5e4
  font-size: 16px
  height: auto
  margin: 0
  outline: 0
  padding: 8px 18px
  height: 48px
  width: 100%
  box-shadow: 0 1px 0 rgba(0,0,0,0.03) inset
  border-radius: 9px
  margin-bottom: 0
  transition: all .2s ease

input::placeholder
  color: #999

input:focus,
select:focus
  border: 2px solid #7fc7ff

input[type="radio"],
input[type="checkbox"]
  margin: 0 4px 8px 0

input[type="radio"],
input[type="checkbox"], select
  cursor: pointer

button
  font-size: 16px
  font-weight: 600
  text-transform: uppercase
  padding: 15px 26px
  color: #FFF
  background-color: #4db2ff
  border-radius: 9px
  text-align: center
  width: 100%
  border: none
  cursor: pointer
  transition: all 0.3s
  &:hover
    background-color: #7fc7ff

label
  display: block
  line-height: 24px
  font-size: 16px
  font-weight: 400
  margin-bottom: 4px
  &.light
    font-weight: 300
    display: inline

.client_group[multiple]
  overflow-y: auto

.client_group
  width: 100%
  height: calc(4 * 38px)
  border: 2px solid #cdd6f3
  border-bottom-right-radius: 2px
  border-bottom-left-radius: 2px
  margin-bottom: 0
  & option
    &:checked
      background-color: #eceff3
    &:hover
      background-color: #d5e8fb
    display: block
    padding: 8px 16px
    cursor: pointer

.is_invalid
  border: 2px solid #ff6347 !important

.invalid
  color: #ff6347
  font-size: 14px
  margin: 0

.hint
  margin: 0
  font-size: 14px

@media screen and (max-width: 650px)
  .hint
    display: none
</style>
