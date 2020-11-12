<template>
  <form class="card auth-card" @submit.prevent="submitHandler">
    <div class="card-content">
      <h1 class="card-title purple-text text-lighten-2">Список дел</h1>
      <div class="input-field">
        <input
            id="email"
            type="text"
            v-model.trim="email"
            :class="{invalid: ($v.email.$dirty && !$v.email.required) || ($v.email.$dirty && !$v.email.email)}"
        >
        <label for="email">Введите Email</label>
        <small
          class="helper-text invalid purple-text text-lighten-2"
          v-if="$v.email.$dirty && !$v.email.required"
        >Поле Email не должно быть пустым</small>
        <small
          class="helper-text invalid purple-text text-lighten-2"
          v-else-if="$v.email.$dirty && !$v.email.email"
        >Введите корретный Email</small>
      </div>
      <div class="input-field">
        <input
            id="password"
            type="password"
            v-model.trim="password"
            :class="{invalid: ($v.password.$dirty && !$v.password.required) || ($v.password.$dirty && !$v.password.minLength)}"
        >
        <label for="password">Введите пароль</label>
        <small
          class="helper-text invalid purple-text text-lighten-2"
          v-if="$v.password.$dirty && !$v.password.required"
        >

        </small>
        <small
          class="helper-text invalid purple-text text-lighten-2"
          v-else-if="$v.password.$dirty && !$v.password.minLength"
        >
          Пароль должен быть {{$v.password.$params.minLength.min}} символов. Сейчас он {{password.length}}
        </small>
      </div>
      <div class="input-field">
        <input
            id="name"
            type="text"
            v-model.trim="name"
            :class="{invalid: $v.name.$dirty && !$v.name.required}"
        >
        <label for="name">Введите ваше имя</label>
        <small
          class="helper-text invalid purple-text text-lighten-2"
          v-if="$v.name.$dirty && !$v.name.required"
        >
        </small>
      </div>
      <p>
        <label>
          <input type="checkbox" v-model="agree" />
          <span>С правилами согласен</span>
        </label>
      </p>
    </div>
    <div class="card-action">
      <div>
        <button
            class="btn waves-effect waves-light auth-submit purple lighten-2"
            type="submit"
        >
          Зарегистрироваться
          <i class="material-icons right">person_pin</i>
        </button>
      </div>

      <p class="center">
        Уже есть аккаунт?
        <router-link to="/login" class="purple-text text-lighten-2">Войти!</router-link>
      </p>
    </div>
  </form>
</template>

<script>
import {email, required, minLength} from 'vuelidate/lib/validators'

export default {
  name: 'register',
  data: () => ({
    email: '',
    password: '',
    name: '',
    agree: false
  }),
  validations: {
    email: {email, required},
    password: {required, minLength: minLength(6)},
    name: {required},
    agree: {checked: v => v}
  },
  methods: {
    async submitHandler() {
      if (this.$v.$invalid) {
        this.$v.$touch()
        return
      }

      const formData = {
        email: this.email,
        password: this.password,
        name: this.name
      }
    try {
      await this.$store.dispatch('register', formData)
      this.$router.push('/')
    } catch (e) {}

    }
  }
}
</script>
