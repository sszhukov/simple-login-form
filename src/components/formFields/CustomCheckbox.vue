<template>
  <div>
    <input :class="$style.customCheckbox" v-model="checkboxStatus" type="checkbox" id="privacy">
    <label for="privacy">
      <p :class="$style.label"><slot></slot></p>
    </label>
  </div>
</template>

<script>
export default {
  props: {
    value: Boolean,
  },
  computed: {
    checkboxStatus: {
      get() {
        return this.value;
      },
      set(value) {
        this.$emit('input', value);
      },
    },
  },
};
</script>

<style lang="scss" module>
  .customCheckbox {
    position: absolute;
    z-index: -1;
    opacity: 0;

    &+label {
      display: inline-flex;
      align-items: center;
      user-select: none;

      &::before {
        content: '';
        display: inline-block;
        width: 1.625rem;
        height: 1.625rem;
        margin-right: .5rem;

        background: $baseBackgroundColor;
        border: 1px solid $nonActiveColor;
        border-radius: 4px;
        box-shadow: $baseBoxShadow;
      }
    }

    &:checked+label::before {
      content: '';
      border: 2px solid $colorPrimary;
      background-image: url('../../assets/svg-icons/check-mark.svg');
      background-repeat: no-repeat;
      background-position: center;
      background-size: 1rem 1rem;
    }
  }

  .label {
    white-space: pre-wrap;
  }
</style>
