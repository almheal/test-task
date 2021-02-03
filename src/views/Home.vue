<template>
  <div class="home">
    <div class="home__inner">
      <Form
        :title="title"
        :btnName="btnName"
        :disabledBtn="getLengthFilledFields === 4 && terms ? false : true"
        @submitForm="registerHandler"
      >
        <div class="login">
          <span>Уже есть аккаунт?</span> <router-link class="form__link" to="/">Войти</router-link>
        </div>
        <div class="form__body">
          <div class="wrapper__input form__input">
            <label class="input__title">Имя</label>
            <input type="text" class="input" placeholder="Введите Ваше имя" v-model="user.name">
          </div>
          <div class="wrapper__input form__input">
            <label class="input__title">Email</label>
            <input type="text" class="input" placeholder="Введите ваш email" v-model="user.email">
            <span class="invalid__message"
              v-if="$v.user.email.$dirty && !$v.user.email.email">Не корректные данные</span>
          </div>
          <div class="wrapper__input form__input">
            <label class="input__title">Номер телефона</label>
            <input type="text" class="input" placeholder="Введите номер телефона" v-model="user.number">
            <span class="invalid__message"
              v-if="
              $v.user.number.$dirty && !$v.user.number.maxLength ||
              $v.user.number.$dirty && !$v.user.number.numeric ||
              $v.user.number.$dirty && !$v.user.number.minLength">Не корректные данные</span>
          </div>
          <Select
            class="form__input"
            :label="select.label"
            :placeholder="select.placeholder"
            :items="languages"
            @selectItem="selectLanguageHandler"
          />
          <div class="wrapper__checkbox form__input">
            <input id="accept-terms" type="checkbox" class="checkbox" @change="acceptTermsHandler"/>
            <label for="accept-terms" class="input__title checkbox-title">Принимаю <router-link class="form__link" to="/">условия</router-link> использования</label>
          </div>
        </div>
      </Form>
    </div>
  </div>
</template>

<script>
import Form from '@/components/Form'
import Select from '@/components/Select'
import {maxLength, minLength, email, numeric, required} from 'vuelidate/lib/validators'

export default {
  name: 'Home',
  components:{
    Form,
    Select
  },
  data: () => ({
    title: 'Регистрация',
    btnName: 'Зарегистрироваться',
    languages: ['Русский','Английский','Китайский','Испанский'],
    select:{
      label: 'Язык',
      placeholder: 'Язык'
    },
    number: '',
    user:{
      name: '',
      email: '',
      number: '',
      language: ''
    },
    terms: false,
    disabled: true
  }),
  computed:{
    getLengthFilledFields(){
      const filledFields = []
      this.$v.user.email.email === true && this.$v.user.email.required ? filledFields.push(true) : ''
      const validName = this.user.name.match(/^[A-Za-z -]+$/)
      if(validName){
        filledFields.push(true)
      }
      const validNumber = this.user.number.match(/^(\s*)?(\+)?([- _():=+]?\d[- _():=+]?){11}(\s*)?$/)
      if(validNumber){
        filledFields.push(true)
      }
      if(this.user.language){
        filledFields.push(true)
      }
      return filledFields.length
    }
  },
  methods:{
    registerHandler(){
      if(this.$v.$invalid){
        this.$v.$touch()
        return
      }
      console.log('Регистрация прошла успешно')
    },
    selectLanguageHandler(language){
      this.select.placeholder = language
      this.user.language = language
    },
    acceptTermsHandler(){
      this.terms = !this.terms
    }
  },
  validations:{
    user:{
      name: {},
      email: {email, required},
      number: {maxLength: maxLength(11), minLength: minLength(11), numeric},
    }
  }
}
</script>

<style lang="scss" scoped>
.home__inner{
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
}

.login{
  font-size: 16px;
  color: #2C2738;
  margin-bottom: 56px;
}

.form__link{
  color: #0880AE;
}

.form__input{
  margin-bottom: 33px;
}

@media(max-width: 420px){
  .login{
    font-size: 14px;
    margin-bottom: 36px;
  }
}
</style>