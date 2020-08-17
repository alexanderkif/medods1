<template>
	<form @submit.prevent="submitCreateUserForm()">

		<header class="row-header">Пользователь</header>

		<div class="row">
			<InputText label="Фамилия" :vld="$v.lastname" :val="(lastname)=>{this.lastname = lastname}" />
			<InputText label="Имя" :vld="$v.firstname" :val="(firstname)=>{this.firstname = firstname}" />
			<InputText label="Отчество" :vld="$v.middlename" :val="(middlename)=>{this.middlename = middlename}" />
		</div>

		<div class="row">
			<InputDate label="Дата рождения" :vld="$v.birthDate"
				:val="(birthDate)=>{this.birthDate = birthDate}" width="6rem" />
			<InputText label="Номер телефона" :vld="$v.phone" pref="+7"
				:val="(phone)=>{this.phone = phone}" width="6rem" />
			<SelectText label="Лечащий врач" :vld="$v.doctor" :val="(doctor)=>{this.doctor = doctor}"
				:values="doctors" />
		</div>

		<div class="row">
			<SelectText label="Группа клиентов" :vld="$v.client" :val="(client)=>{this.client = client}"
				:values="clients" :mult="true" />
			<div class="row-item">
				<label class="row-item__label">Пол</label>
				<input class="radio" type="radio" value="male" v-model="gender">
				<label for="one">Мужской</label>
				<br>
				<input class="radio" type="radio" value="female" v-model="gender">
				<label for="two">Женский</label>
			</div>
			<div class="row-item">
				<input class="checkbox" type="checkbox" v-model="sendSMS">
				<label for="checkbox">Не отправлять СМС</label>
			</div>
		</div>

		<header class="row-header">Адрес</header>

		<div class="row">
			<InputText label="Индекс" :vld="$v.stateIndex" :val="(stateIndex)=>{this.stateIndex = stateIndex}" />
			<InputText label="Страна" :vld="$v.country" :val="(country)=>{this.country = country}" />
			<InputText label="Область" :vld="$v.stateName" :val="(stateName)=>{this.stateName = stateName}" />
			<InputText label="Город" :vld="$v.sity" :val="(sity)=>{this.sity = sity}" />
			<InputText label="Улица" :vld="$v.street" :val="(street)=>{this.street = street}" />
			<InputText label="Дом" :vld="$v.building" :val="(building)=>{this.building = building}" />
		</div>

		<header class="row-header">Паспорт</header>

		<div class="row">
			<SelectText label="Тип документа" :vld="$v.docType" :val="(docType)=>{this.docType = docType}"
				:values="docTypes" />
			<InputText label="Серия" :vld="$v.docSer" :val="(docSer)=>{this.docSer = docSer}" width="5rem" />
			<InputText label="Номер" :vld="$v.docNumber" :val="(docNumber)=>{this.docNumber = docNumber}" width="7rem" />
			<InputDate label="Дата выдачи" :vld="$v.docDate"
				:val="(docDate)=>{this.docDate = docDate}" width="6rem" />
			<InputText label="Кем выдан" :vld="$v.docCreator" allow="customSpaceAlpha"
				:val="(docCreator)=>{this.docCreator = docCreator}" width="15rem" />
		</div>

		<div class="row">
			<p class="typo__p" v-if="submitStatus === 'OK'">Пользователь успешно создан!</p>
			<p class="typo__p row-item__group_error" v-if="submitStatus === 'ERROR'">Пожалуйста заполните обязательные поля.</p>
			<p class="typo__p" v-if="submitStatus === 'PENDING'">Отправляем...</p>
		</div>

		<div class="row">
			<button class="submit-button" type="submit" :disabled="submitStatus === 'PENDING'">
				Создать пользователя
			</button>
		</div>
	</form>     
</template>

<script>
import { required, minLength, maxLength, numeric } from 'vuelidate/lib/validators'
import moment from 'moment'
import InputText from './InputText'
import InputDate from './InputDate'
import SelectText from './SelectText'

export default {
	name: 'CreateUser',
	components: {
		InputText,
		InputDate,
		SelectText
	},
  data() {
    return {
      lastname: '',
      firstname: '',
			middlename: '',
			birthDate: '',
			phone: '',
			gender: '',
			client: [],
			clients: ['VIP', 'Проблемные', 'ОМС'],
			doctor: '',
			doctors: ['Иванов', 'Захаров', 'Чернышева'],
			sendSMS: false,
			stateIndex: '',
			country: '',
			stateName: '',
			sity: '',
			street: '',
			building: '',
			docType: '',
			docTypes: ['Паспорт', 'Свидетельство о рождении', 'Вод. удостоверение'],
			docSer: '',
			docNumber: '',
			docCreator: '',
			docDate: '',
			user: {},
			submitStatus: null
    }
	},
  validations: {
    lastname: {
      required,
      minLength: minLength(2),
      customAlpha: val => /^[а-яёa-z]*$/i.test(val),
    },
    firstname: {
      required,
      minLength: minLength(2),
      customAlpha: val => /^[а-яёa-z]*$/i.test(val),
		},
		middlename: {
      minLength: minLength(2),
			customAlpha: val => /^[а-яёa-z]*$/i.test(val)
		},
		birthDate: {
      required,
      validDate: val => moment(val, 'DD.MM.YYYY', true).isValid(),
    },
    phone: {
      required,
			numeric,
      minLength: minLength(10),
      maxLength: maxLength(10),
		},
		doctor: {},
		client: {
			required,
		},
    stateIndex: {
      numeric,
      minLength: minLength(6),
      maxLength: maxLength(6),
    },
    country: {
      minLength: minLength(2),
      customAlpha: val => /^[а-яёa-z]*$/i.test(val)
    },
    stateName: {
      minLength: minLength(2),
      customAlpha: val => /^[а-яёa-z]*$/i.test(val)
    },
    sity: {
      required,
      minLength: minLength(2),
      customAlpha: val => /^[а-яёa-z]*$/i.test(val)
    },
    street: {
      minLength: minLength(2),
      customAlpha: val => /^[а-яёa-z]*$/i.test(val)
    },
    building: {
      numeric,
		},
		docType: {
      required,
    },
    docSer: {
			numeric,
			minLength: minLength(4),
      maxLength: maxLength(4)
		},
    docNumber: {
      numeric,
			minLength: minLength(6),
      maxLength: maxLength(6)
		},
    docCreator: {
      minLength: minLength(2),
      customSpaceAlpha: val => /^[а-яёa-z\s]*$/i.test(val)
		},
		docDate: {
      required,
      validDate: val => moment(val, 'DD.MM.YYYY', true).isValid(),
    },
  },
  methods: {
		submitCreateUserForm() {
      this.$v.$touch()
      if (this.$v.$invalid) {
        this.submitStatus = 'ERROR'
      } else {
        this.submitStatus = 'PENDING'
				this.user = {}
				this.user.lastname = this.lastname
				this.user.firstname = this.firstname
				this.user.middlename = this.middlename
				this.user.birthDate = this.birthDate
				this.user.phone = this.phone
				this.user.gender = this.gender
				this.user.client = this.client
				this.user.doctor = this.doctor
				this.user.sendSMS = this.sendSMS
				this.user.address = {}
				this.user.address.stateIndex = this.stateIndex
				this.user.address.country = this.country
				this.user.address.stateName = this.stateName
				this.user.address.sity = this.sity
				this.user.address.street = this.street
				this.user.address.building = this.building
				this.user.document = {}
				this.user.document.docType = this.docType
				this.user.document.docSer = this.docSer
				this.user.document.docNumber = this.docNumber
				this.user.document.docCreator = this.docCreator
				this.user.document.docDate = this.docDate
				console.log(this.user)
        setTimeout(() => {
          this.submitStatus = 'OK'
					alert(`Создан пользователь: ${JSON.stringify(this.user)}`)
				}, 500)
      }
		}
  }
}
</script>

<style lang="sass" >
$error-color: #f79483
$document-width: 43rem
$light-grey: #f8f8f8
$dark-grey: #555

body
	margin: 0

.row-header
	background-color: $dark-grey
	color: $light-grey
	max-width: $document-width
	margin: 0 auto
	padding: 1rem 1rem 0.5rem 1rem
	font-size: 1.2rem
	text-align: left

.row
	display: flex
	flex-wrap: wrap
	justify-content: left
	max-width: $document-width
	margin: 0 auto
	padding: 0 1rem 0 1rem
	font-size: 0.9rem
	background-color: $light-grey
	justify-content: space-around
	color: $dark-grey

.row-item
  padding: 0.5rem

  &__label
    display: block
    text-align: left
    font-size: 0.7rem
    padding-left: 0.3rem

  &__error
    color: red
    font-size: 0.7rem
    color: $error-color
    min-width: max-content

  &__group
    &_error
      color: $error-color
      animation-name: shakeError
      animation-fill-mode: forwards
      animation-duration: .6s
      animation-timing-function: ease-in-out

  &__input
    border: 0.05rem solid
    border-radius: 0.2rem
    transition: border .1s ease
    box-sizing: border-box
    padding-left: 0.3rem
    color: $dark-grey
    
    &_error
      border-color: $error-color

    &:focus
      outline: none

.submit-button
	margin: 0 auto 1rem
	color: $dark-grey

.typo__p
	margin: 0 auto

@keyframes shakeError
  0%
    transform: translateX(0)
  15%
    transform: translateX(0.375rem)
  30%
    transform: translateX(-0.375rem)
  45%
    transform: translateX(0.375rem)
  60%
    transform: translateX(-0.375rem)
  75%
    transform: translateX(0.375rem)
  90%
    transform: translateX(-0.375rem)
  100%
    transform: translateX(0)

</style>
