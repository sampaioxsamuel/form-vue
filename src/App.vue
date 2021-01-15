<template>
  <div id="app"  class="container-fluid p-5">
    <Form :options="form" :error="error" @data="formData = $event" />
  </div>
</template>

<script>
import Form from './components/Form.vue'

export default {
  name: 'App',
  components: {
    Form
  },
  data() {
    return {
      formData: [],
      form: [{id: 0, placeholder: 'Telefone', type: 'text', icon: 'fa-phone', mask: '(##) #####-####' },
             {id: 1, placeholder: 'Email', type: 'email', icon: 'fa-user'},
             {id: 2, placeholder: 'Senha', type: 'password', icon: 'fa-lock'},
             {id: 3, placeholder: 'Escolha...', type: 'select', selectOptions: [{value:'branco', label:'Branco'}, {value:'azul', label:'Azul'}]}
      ],
      error: false
    }
  }
}
</script>

<style>

</style>
