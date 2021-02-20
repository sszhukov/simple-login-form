<template>
  <div>
    <slot></slot>
    <input v-model="inputValue"
      :class="$style.input"
      :placeholder="placeholder"
    >
    <div v-show="dirty" :class="$style.error">
      <p v-if="error">
        {{ error }}
      </p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    value: String,
    dirty: Boolean,
    placeholder: String,
    errors: Array,
  },
  computed: {
    inputValue: {
      get() {
        return this.value;
      },
      set(value) {
        this.$emit('input', value);
      },
    },
    error() {
      return this.errors.find((error) => error);
    },
  },
};
</script>

<style lang="scss" module>
  .input {
    @include baseFormField;

    &::placeholder {
      color: $colorPlaceholderText;
    }

    &:focus {
      border: 2px solid $colorPrimary;
      padding: calc(#{$formFieldPadding} - 1px);
    }
  }

  .error {
    height: 1rem;
    margin: -1.875rem 0 .875rem 0;
    color: $errorColor;
    font-size: .875rem;
    font-weight: 400;
  }
</style>
