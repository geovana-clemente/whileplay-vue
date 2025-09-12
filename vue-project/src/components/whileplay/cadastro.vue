<template>
  <LayoutContainer>
    <h1>Cadastro</h1>
    <p>Crie sua conta e comece agora!</p>
    <CadastroForm @submit="handleSubmit" />
    <LoginLink text="Já tem conta?" url="/login" />
    <div v-if="errorMessage" class="error-message">{{ errorMessage }}</div>
  </LayoutContainer>
</template>

<script>
import CadastroForm from '../components/CadastroForm.vue';
import LoginLink from '../components/LoginLink.vue';
import LayoutContainer from '../components/LayoutContainer.vue';

export default {
  components: {
    CadastroForm,
    LoginLink,
    LayoutContainer
  },
  data() {
    return {
      errorMessage: ''
    };
  },
  methods: {
    handleSubmit(formData) {
      // Simulate form submission and error handling
      const { nome, email, password, password_confirm } = formData;

      if (password !== password_confirm) {
        this.errorMessage = 'As senhas não coincidem!';
        return;
      }

      // Here you would typically send the data to your backend
      // For now, we will just log it
      console.log('Form submitted:', { nome, email, password });
      // Reset error message on successful submission
      this.errorMessage = '';
    }
  },
  mounted() {
    const params = new URLSearchParams(window.location.search);
    if (params.get('erro')) {
      switch (params.get('erro')) {
        case 'senhas':
          this.errorMessage = 'As senhas não coincidem!';
          break;
        case 'email':
          this.errorMessage = 'Email já cadastrado!';
          break;
        case 'bd':
          this.errorMessage = 'Erro ao cadastrar usuário. Tente novamente.';
          break;
        case 'nome':
          this.errorMessage = 'Digite um nome válido (mínimo 3 letras).';
          break;
        case 'emailinvalido':
          this.errorMessage = 'Digite um email válido.';
          break;
        case 'senhapequena':
          this.errorMessage = 'A senha deve ter pelo menos 6 caracteres.';
          break;
      }
    }
  }
};
</script>

<style scoped>
.error-message {
  color: red;
  margin-top: 1rem;
}
</style>