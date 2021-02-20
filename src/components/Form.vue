<template>
  <form :class="$style.orderForm" @submit.prevent="submit">
    <CustomInput id="name"
               v-model.trim="$v.name.$model"
               placeholder="Введите ваше имя"
               :dirty="$v.name.$dirty"
               :errors="[
                !$v.name.required ? 'Это обязательное поле.': '',
                !$v.name.nameValidateMethod ? 'Введено не корректное значение.': '',
               ]"
    >
      <label :class="$style.label" for="name">Имя</label>
    </CustomInput>

    <CustomInput id="email"
               v-model.trim="$v.email.$model"
               placeholder="Введите ваш email"
               :dirty="$v.email.$dirty"
               :errors="[
                !$v.email.required ? 'Это обязательное поле.': '',
                !$v.email.email ? 'Введено не корректное значение.': '',
               ]"
    >
      <label :class="$style.label" for="email">Email</label>
    </CustomInput>

    <CustomInput id="phone"
               v-model.trim="$v.phone.$model"
               placeholder="Введите ваш номер телефона"
               :dirty="$v.phone.$dirty"
               :errors="[
                !$v.phone.required ? 'Это обязательное поле.': '',
                !$v.phone.phoneValidateMethod ? 'Введено не корректное значение.': '',
               ]"
    >
       <label :class="$style.label" for="phone">Номер телефона</label>
    </CustomInput>

    <label :class="$style.label" for="language">Язык</label>
    <CustomSelect v-model="$v.language.$model" :options="languages"/>

    <CustomCheckbox v-model="$v.privacyChecked.$model">
      Принимаю <a href="#">условия</a> использования
    </CustomCheckbox>

    <button :disabled="!formFilled" :class="[$style.submitButton, formFilled ? $style.submitButton_active : $style.submitButton_nonActive]">
      {{ submitButtonText }}
    </button>
  </form>
</template>

<script>
import { validationMixin } from 'vuelidate';
import { required, email, helpers } from 'vuelidate/lib/validators';
import CustomSelect from '@/components/formFields/CustomSelect.vue';
import CustomInput from '@/components/formFields/CustomInput.vue';
import CustomCheckbox from '@/components/formFields/CustomCheckbox.vue';

const nameValidateMethod = helpers.regex('name', /^[a-zа-я\- ]*$/i);
const phoneValidateMethod = helpers.regex('phone', /^[0-9+\-()]*$/);
const checkedValidateMethod = (value) => !!value;

export default {
  components: { CustomSelect, CustomInput, CustomCheckbox },
  mixins: [validationMixin],
  data() {
    return {
      name: '',
      email: '',
      phone: '',
      language: '',
      privacyChecked: false,
      submitStatus: 'lock',

      languages: [
        'Русский',
        'Английский',
        'Китайский',
        'Испанский',
      ],
      submitButtonText: 'Зарегистрироваться',
    };
  },
  computed: {
    formFilled() {
      return !this.$v.$invalid;
    },
  },
  validations: {
    name: {
      required,
      nameValidateMethod,
    },
    email: {
      required,
      email,
    },
    phone: {
      required,
      phoneValidateMethod,
    },
    language: {
      required,
    },
    privacyChecked: {
      checkedValidateMethod,
    },
  },
  methods: {
    submit() {
      this.$v.$touch();
      if (!this.$v.$invalid) {
        this.submitStatus = 'SUCCESS';
        const orderData = {
          name: this.name,
          email: this.email,
          phone: this.phone,
          language: this.language,
        };
        this.name = '';
        this.email = '';
        this.phone = '';
        this.language = '';
        this.privacyChecked = false;
        this.$v.$reset();
        this.submitStatus = 'SUCCESS';
        this.$emit('submit', orderData);
      }
    },
  },
};
</script>

<style lang="scss" module>
  .orderForm {
    margin-top: 60px;

    .label {
      display: inline-block;
      color: $colorSecondaryText;
      font-weight: 500;
      margin-bottom: 7px;
    }

    .submitButton {
      width: 100%;
      margin-top: 40px;
      padding: $formFieldPadding;
      font-size: 1rem !important;
      font-weight: 500 !important;
      border-radius: 6px;
      outline: none;
    }

    .submitButton_active {
      color: $baseBackgroundColor;
      background: $colorPrimary;
      box-shadow: $baseBoxShadow, 0px 4px 8px rgba(44, 39, 56, 0.08);

      &:active {
        padding: calc(#{$formFieldPadding} - 2px);
        border: 2px solid $colorTextActive;
        box-shadow: 0px 2px 4px rgba(44, 39, 56, 0.0001), $baseBoxShadow;
      }

      &:hover {
        box-shadow: 0px 12px 24px rgba(44, 39, 56, 0.08), 0px 24px 48px rgba(44, 39, 56, 0.16);
      }
    }

    .submitButton_nonActive {
      color: $colorSecondaryText;
      background: $nonActiveColor;
      box-shadow: $baseBoxShadow;
    }
  }
</style>
