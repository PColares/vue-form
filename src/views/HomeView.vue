<script setup>
import BaseInput from '../components/BaseInput.vue'
import { ref, reactive, computed } from 'vue'
import useVuelidate from '@vuelidate/core'
import { required, minLength, email, sameAs, helpers } from '@vuelidate/validators'

// custom validations
const containsUser = (value) => {
  return value.includes('_')
}

const formData = reactive({
  username: '',
  email: '',
  password: '',
  confirmPassword: ''
})

const rules = computed(() => {
  return {
    // custom validation messages with helpers.message
    username: {
      required: helpers.withMessage('Campo obrigatório', required),
      minLength: helpers.withMessage('Mínimo de 10 caracteres',minLength(10)),
      containsUser: helpers.withMessage('Deve conter _', containsUser)
    },
    email: { required: helpers.withMessage('Campo obrigatório', required), email },
    password: { required: helpers.withMessage('Campo obrigatório', required)},
    confirmPassword: { required: helpers.withMessage('Campo obrigatório', required), sameAs: helpers.withMessage('Senhas devem ser iguais', sameAs(formData.password)) }
  }
})

const v$ = useVuelidate(rules, formData)

const submitForm = async () => {
  const result = await v$.value.$validate()
  if (result) {
    console.log('Success, form submitted !')
    console.log('vif', v$)
  } else {
    console.log('error, form not submitted !')
    console.log('v$.value', v$)
  }
}
</script>

<template>
  <body>
    <pre>{{$v}}</pre>
    <form @submit.prevent="submitForm">
      <div class="div-wrap">
        <BaseInput label="Usuário" v-model="formData.username" type="text" />
          <span v-for="x in v$.username.$errors" :key="x.$uid">
            {{x.message}}
          </span>

      </div>

      <div class="div-wrap">
        <BaseInput label="E-mail" v-model="formData.email" type="email" />
        <span v-for="error in v$.email.$errors" :key="error.$uid">{{ error.$message }}</span>
      </div>

      <div class="div-wrap">
        <BaseInput label="Senha" v-model="formData.password" type="password" />
        <span v-for="error in v$.password.$errors" :key="error.$uid">{{ error.$message }}</span>
      </div>
      <div class="div-wrap">
        <BaseInput label="Confirmar Senha" v-model="formData.confirmPassword" type="password" />
        <span v-for="error in v$.confirmPassword.$errors" :key="error.$uid">{{error.$message}}</span>
      </div>

      <button type="submit">Criar usuário</button>
    </form>
    <!-- grouping errors -->
    <!-- <div>
      <p>Errors:</p>
      <span v-for="error in v$.$errors" :key="error.$uid">
        {{ error.$property }} - {{ error.$message }}
      </span>
    </div> -->
  </body>
</template>

<style scoped>
body {
  background-color: lightslategrey;
  height: 500px;
  padding: 2rem;
  border-radius: 6px;
  color: white;
}

span {
  flex-direction: row;
  line-height: 2;
  color: lightsalmon;
  height: 30px;
  margin-right: 0.5rem;
}

.div-wrap {
  height: 100px;
}
</style>
