<template>
  <div id="app">
    <h1>Регистрация</h1>
    <p>Уже есть аккаунт? <a href="#">Войти</a></p>
    <Form @submit="submit"/>
  </div>
</template>

<script>
import Form from '@/components/Form.vue';

export default {
  components: { Form },
  name: 'App',
  methods: {
    submit(formData) {
      console.log('Submit success', formData);
    },
  },
};
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=IBM+Plex+Sans:wght@400;500;700&display=swap');

*, *::before, *::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  border: none;
}

html {
  background: #e5e5e5;
}

#app {
  display: flex;
  flex-direction: column;
  min-width: 360px;
  width: 100vw;
  padding: 30px 40px;
  background: $baseBackgroundColor;
  box-shadow: 0px 12px 24px rgba(44, 39, 56, 0.02), $baseBoxShadow;
  border-radius: 24px;
  @include baseFont;

  input, button {
    @include baseFont;
  }
}
</style>
