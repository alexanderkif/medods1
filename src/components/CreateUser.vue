<template>
	<form @submit.prevent="submitCreateUserForm()">

		<header class="row-header">Пользователь</header>

		<div class="row">
			<div class="row-item">
				<div class="form-group" :class="{ 'form-group--error': $v.lastname.$error }">
					<label class="form__label">Фамилия *</label>
					<input class="form__input" v-model.trim="lastname" @input="setLastname($event.target.value)" />
				</div>
				<div class="error" v-if="lastname.length&&!$v.lastname.required">Поле обязательное для заполнения</div>
				<div class="error" v-else-if="lastname.length&&!$v.lastname.customAlpha">В фамилии должны быть только буквы</div>
				<div class="error" v-else-if="lastname.length&&!$v.lastname.minLength">Фамилия минимум {{$v.lastname.$params.minLength.min}} буквы.</div>
			</div>

			<div class="row-item">
				<div class="form-group" :class="{ 'form-group--error': $v.firstname.$error }">
					<label class="form__label">Имя *</label>
					<input class="form__input" v-model.trim="firstname" @input="setFirstname($event.target.value)" />
				</div>
				<div class="error" v-if="firstname.length&&!$v.firstname.required">Поле обязательное для заполнения</div>
				<div class="error" v-else-if="firstname.length&&!$v.firstname.customAlpha">В имени должны быть только буквы</div>
				<div class="error" v-else-if="firstname.length&&!$v.firstname.minLength">Фамилия минимум {{$v.firstname.$params.minLength.min}} буквы.</div>
			</div>

			<div class="row-item">
				<div class="form-group" >
					<label class="form__label">Отчество</label>
					<input class="form__input" v-model.trim="middlename" @input="setMiddlename($event.target.value)" />
				</div>
				<div class="error" v-if="middlename.length&&!$v.middlename.customAlpha">В отчестве должны быть только буквы</div>
				<div class="error" v-else-if="middlename.length&&!$v.middlename.minLength">Отчество минимум {{$v.middlename.$params.minLength.min}} буквы.</div>
			</div>
		</div>

		<div class="row">
			<div class="row-item">
				<div class="form-group" :class="{ 'form-group--error': $v.birthDate.$error }">
					<label class="form__label">Дата рождения *</label>
					<input class="form__input date" v-model.trim="birthDate" @change="setBirthDate($event.target.value)" placeholder="31.12.2020"/>
				</div>
				<div class="error" v-if="birthDate.length&&!$v.birthDate.required">Поле обязательное для заполнения</div>
				<div class="error" v-else-if="birthDate.length&&!$v.birthDate.validDate">Такой даты не существует</div>
			</div>
			<div class="row-item">
				<div class="form-group" :class="{ 'form-group--error': $v.phone.$error }">
					<label class="form__label">Номер телефона *</label>
					+7<input class="form__input phone" v-model="phone" @input="setPhone($event.target.value)" placeholder="2233223232"/>
				</div>
				<div class="error" v-if="phone.length&&!$v.phone.numeric">В номере должны быть только цифры</div>
				<div class="error" v-if="phone.length&&!$v.phone.valLength">Номер должен быть 11 цифр</div>
			</div>

			<div class="row-item">
				<label class="form__label">Лечащий врач</label>
				<select v-model="doctor" @change="setDoctor">
					<!-- <option disabled value="">Лечащий врач</option> -->
					<option
						v-for="doctor in doctors"
						:key="doctor.index"
						>{{ doctor }}</option>
				</select>
			</div>
		</div>

		<div class="row">
			<div class="row-item">
				<div class="form-group" :class="{ 'form-group--error': $v.client.$error }">
					<label class="form__label">Группа клиентов *</label>
					<select v-model="client" multiple :size="clients.length">
						<option
							v-for="client in clients"
							:key="client.index"
							>{{ client }}</option>
					</select>
				</div>
				<div class="error" v-if="client.length&&!$v.client.required">Поле обязательное для заполнения</div>
			</div>

			<div class="row-item">
				<label class="form__label">Пол</label>
				<input class="radio" type="radio" value="male" v-model="gender">
				<label for="one">Мужской</label>
				<br>
				<input class="radio" type="radio" value="female" v-model="gender">
				<label for="two">Женский</label>
			</div>
		</div>

		<div class="row">
			<div class="row-item">
				<input class="checkbox" type="checkbox" v-model="sendSMS">
				<label for="checkbox">Не отправлять СМС</label>
			</div>
		</div>

		<header class="row-header">Адрес</header>

		<div class="row">
			<div class="row-item">
				<div class="form-group" :class="{ 'form-group--error': $v.stateIndex.$error }">
					<label class="form__label">Индекс</label>
					<input class="form__input" v-model.trim="stateIndex" @input="setStateIndex($event.target.value)" />
				</div>
				<div class="error" v-if="stateIndex.length&&!$v.stateIndex.numbers">В индексе должны быть только цифры</div>
				<div class="error" v-else-if="stateIndex.length&&!$v.stateIndex.minLength">В индексе {{$v.stateIndex.$params.minLength.min}} цифр.</div>
				<div class="error" v-else-if="stateIndex.length&&!$v.stateIndex.maxLength">В индексе {{$v.stateIndex.$params.maxLength.max}} цифр.</div>
			</div>

			<div class="row-item">
				<div class="form-group" :class="{ 'form-group--error': $v.country.$error }">
					<label class="form__label">Страна</label>
					<input class="form__input" v-model.trim="country" @input="setCountry($event.target.value)" />
				</div>
				<div class="error" v-if="country.length&&!$v.country.customAlpha">В стране должны быть только буквы</div>
				<div class="error" v-else-if="country.length&&!$v.country.minLength">В стране не менее {{$v.country.$params.minLength.min}} букв.</div>
			</div>

			<div class="row-item">
				<div class="form-group" :class="{ 'form-group--error': $v.stateName.$error }">
					<label class="form__label">Область</label>
					<input class="form__input" v-model.trim="stateName" @input="setStateName($event.target.value)" />
				</div>
				<div class="error" v-if="stateName.length&&!$v.stateName.customAlpha">В области должны быть только буквы</div>
				<div class="error" v-else-if="stateName.length&&!$v.stateName.minLength">В области не менее {{$v.stateName.$params.minLength.min}} букв.</div>
			</div>

			<div class="row-item">
				<div class="form-group" :class="{ 'form-group--error': $v.sity.$error }">
					<label class="form__label">Город *</label>
					<input class="form__input" v-model.trim="sity" @input="setSity($event.target.value)" />
				</div>
				<div class="error" v-if="sity.length&&!$v.sity.required">Поле обязательное для заполнения</div>
				<div class="error" v-else-if="sity.length&&!$v.sity.customAlpha">В городе должны быть только буквы</div>
			</div>

			<div class="row-item">
				<div class="form-group" :class="{ 'form-group--error': $v.street.$error }">
					<label class="form__label">Улица</label>
					<input class="form__input" v-model.trim="street" @input="setStreet($event.target.value)" />
				</div>
				<div class="error" v-if="street.length&&!$v.street.customAlpha">В улице должны быть только буквы</div>
				<div class="error" v-else-if="street.length&&!$v.street.minLength">В улице не менее {{$v.street.$params.minLength.min}} букв.</div>
			</div>

			<div class="row-item">
				<div class="form-group" :class="{ 'form-group--error': $v.building.$error }">
					<label class="form__label">Дом</label>
					<input class="form__input" v-model.trim="building" @input="setBuilding($event.target.value)" />
				</div>
				<div class="error" v-if="building.length&&!$v.building.numbers">В доме должны быть только цифры</div>
			</div>
		</div>

		<header class="row-header">Паспорт</header>

		<div class="row">
			<div class="row-item">
				<div class="form-group" :class="{ 'form-group--error': $v.docType.$error }">
					<label class="form__label">Тип документа *</label>
					<select v-model="docType" >
						<option
							v-for="docType in docTypes"
							:key="docType.index"
							>{{ docType }}</option>
					</select>
				</div>
				<div class="error" v-if="docType.length&&!$v.docType.required">Поле обязательное для заполнения</div>
			</div>

			<div class="row-item">
				<div class="form-group" :class="{ 'form-group--error': $v.docSer.$error||$v.docNumber.$error }">
					<label class="form__label">Серия-номер документа</label>
					<input class="form__input doc-ser" v-model="docSer" @input="setDocSer($event.target.value)" placeholder="9876"/>
					-
					<input class="form__input doc-number" v-model="docNumber" @input="setDocNumber($event.target.value)" placeholder="543210" />
				</div>
				<div class="error" v-if="docSer.length&&!$v.docSer.numeric">В серии должны быть только цифры</div>
				<div class="error" v-else-if="docSer.length&&!$v.docSer.valLength">Серия должна быть 4 цифры</div>
				<div class="error" v-else-if="docNumber.length&&!$v.docNumber.numeric">В номере должны быть только цифры</div>
				<div class="error" v-else-if="docNumber.length&&!$v.docNumber.valLength">Номер должен быть 6 цифр</div>
			</div>

			<div class="row-item">
				<div class="form-group" :class="{ 'form-group--error': $v.docDate.$error }">
					<label class="form__label">Дата выдачи *</label>
					<input class="form__input date" v-model.trim="docDate" @change="setDocDate($event.target.value)" placeholder="31.12.2020"/>
				</div>
				<div class="error" v-if="docDate.length&&!$v.docDate.required">Поле обязательное для заполнения</div>
				<div class="error" v-else-if="docDate.length&&!$v.docDate.validDate">Такой даты не существует</div>
			</div>

			<div class="row-item">
				<div class="form-group" :class="{ 'form-group--error': $v.docCreator.$error }">
					<label class="form__label">Кем выдан</label>
					<input class="form__input doc-creator" v-model="docCreator" @input="setDocCreator($event.target.value)" />
				</div>
				<div class="error" v-if="docCreator.length&&!$v.docCreator.customAlphaSpace">В области должны быть только буквы</div>
				<div class="error" v-else-if="docCreator.length&&!$v.docCreator.minLength">В области не менее {{$v.docCreator.$params.minLength.min}} букв.</div>
			</div>
		</div>

		<div class="row">
			<p class="typo__p" v-if="submitStatus === 'OK'">Пользователь успешно создан!</p>
			<p class="typo__p form-group--error" v-if="submitStatus === 'ERROR'">Пожалуйста заполните обязательные поля.</p>
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

export default {
	name: 'CreateUser',
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
			doctors: ['Иванов', 'Захаров', 'Чернышева', 'Очистить'],
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
      customAlpha: val => /^[а-яё,a-z]*$/i.test(val),
    },
    firstname: {
      required,
      minLength: minLength(2),
      customAlpha: val => /^[а-яё,a-z]*$/i.test(val),
		},
		middlename: {
      minLength: minLength(2),
			customAlpha: val => /^[а-яё,a-z]*$/i.test(val)
		},
		birthDate: {
      required,
      validDate: val => moment(val, 'DD.MM.YYYY', true).isValid(),
    },
    phone: {
			numeric,
			valLength: val => val.length === 10
		},
		client: {
			required,
		},
    stateIndex: {
      minLength: minLength(6),
      maxLength: maxLength(6),
      numbers: val => /^[0-9]*$/i.test(val),
    },
    country: {
      minLength: minLength(2),
      customAlpha: val => /^[а-яё,a-z]*$/i.test(val)
    },
    stateName: {
      minLength: minLength(2),
      customAlpha: val => /^[а-яё,a-z]*$/i.test(val)
    },
    sity: {
      required,
      minLength: minLength(2),
      customAlpha: val => /^[а-яё,a-z]*$/i.test(val)
    },
    street: {
      minLength: minLength(2),
      customAlpha: val => /^[а-яё,a-z]*$/i.test(val)
    },
    building: {
      numbers: val => /^[0-9]*$/i.test(val),
		},
		docType: {
      required,
    },
    docSer: {
			numeric,
			valLength: val => !val.length || val.length === 4
		},
    docNumber: {
      numeric,
			valLength: val => !val.length || val.length === 6
		},
    docCreator: {
      minLength: minLength(2),
      customAlphaSpace: val => /^[а-яё,a-z,\s]*$/i.test(val)
		},
		docDate: {
      required,
      validDate: val => moment(val, 'DD.MM.YYYY', true).isValid(),
    },
  },
  methods: {
    setLastname(value) {
      this.lastname = value.toUpperCase()
      this.$v.lastname.$touch()
    },
    setFirstname(value) {
      this.firstname = value.toUpperCase()
      this.$v.firstname.$touch()
    },
    setMiddlename(value) {
      this.middlename = value.toUpperCase()
      this.$v.middlename.$touch()
		},
    setBirthDate(value) {
      this.birthDate = value
      this.$v.birthDate.$touch()
    },
    setPhone(value) {
      this.phone = value
      this.$v.phone.$touch()
    },
		setDoctor() {
			if (this.doctor === 'Очистить') this.doctor = ''
		},
    setStateIndex(value) {
      this.stateIndex = value
      this.$v.stateIndex.$touch()
    },
    setCountry(value) {
      this.country = value.toUpperCase()
      this.$v.country.$touch()
    },
    setStateName(value) {
      this.stateName = value.toUpperCase()
      this.$v.stateName.$touch()
    },
    setSity(value) {
      this.sity = value.toUpperCase()
      this.$v.sity.$touch()
    },
    setStreet(value) {
      this.street = value.toUpperCase()
      this.$v.street.$touch()
    },
    setBuilding(value) {
      this.building = value
      this.$v.building.$touch()
    },
    setDocType(value) {
      this.docType = value
      this.$v.docType.$touch()
    },
    setDocSer(value) {
      this.docSer = value
      this.$v.docSer.$touch()
    },
    setDocNumber(value) {
      this.docNumber = value
      this.$v.docNumber.$touch()
    },
    setDocCreator(value) {
      this.docCreator = value.toUpperCase()
      this.$v.docCreator.$touch()
    },
    setDocDate(value) {
      this.docDate = value
      this.$v.docDate.$touch()
    },
		submitCreateUserForm() {
      this.$v.$touch()
      if (this.$v.$invalid) {
        this.submitStatus = 'ERROR'
      } else {
        // do your submit logic here
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
	padding: 1rem 1rem 0 1rem
	font-size: 0.9rem
	background-color: $light-grey
	justify-content: space-around
	color: $dark-grey

	.row-item
		padding: 0 0.5rem
		margin-bottom: 1rem
		position: relative

		.form__label
			display: block
			text-align: left
			font-size: 0.7rem
			padding-left: 0.3rem

		input
			min-width: 13rem

		.doc-ser
			min-width: 3rem
			width: 3rem

		.doc-number
			min-width: 5rem
			width: 5rem

		.date
			min-width: 6rem
			width: 6rem

		.phone
			min-width: 6rem
			width: 6rem
			margin-left: 0.2rem

		.doc-creator
			width: 18rem

		select
			min-width: 8rem

		.radio,
		.checkbox
			min-width: 1rem
			width: 1rem
			margin-right: 1rem

		.error
			position: absolute
			left: 1.3rem
			top: 2.2rem
			color: red
			font-size: 0.7rem
			color: $error-color

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

.form-group--alert,
.form-group--error
  animation-name: shakeError
  animation-fill-mode: forwards
  animation-duration: .6s
  animation-timing-function: ease-in-out

.form-group--alert,
.form-group--error
	color: $error-color

	input,
	select
		border-color: $error-color

input,
select
	border: 0.05rem solid
	border-radius: 0.2rem
	transition: border .1s ease
	box-sizing: border-box
	padding-left: 0.3rem
	color: $dark-grey

input:focus,
select:focus
	outline: none

</style>
