<template>
  <div class="row-item">
    <div class="row-item__group" :class="{ 'row-item__group_error': vld.$error }">
      <label class="row-item__label">{{ label }}{{ vld.$params.required ? ' *' : ''}}</label>
      <input :class="vld.$error ? 'row-item__input row-item__input_error' : 'row-item__input'"
        v-model="value" @input="setValue($event.target.value)"
        :style="`width:${width};`" />
    </div>
    <div class="row-item__error" v-if="vld.$params.required&&value.length&&!vld.required">
      Поле обязательное для заполнения</div>
    <div class="row-item__error" v-else-if="('validDate' in vld.$params)&&value.length&&!vld.validDate">
      Такой даты не существует</div>
  </div>
</template>

<script>
export default {
  name: "InputDate",
  props: {
    label: String,
    vld: Object,
    val: Function,
    width: {
      type: String,
      default: '11rem'
    }
  },
  data() {
    return {
      value: ''
    }
  },
  methods: {
    setValue(value) {
      this.val(value)
      this.vld.$touch()
    }
  }
}
</script>
