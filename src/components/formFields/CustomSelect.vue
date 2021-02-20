<template>
  <div class="select">
    <button type="button" class="trigger" :class="{'down': showingOptions}" @click="showOptions">
      <span v-if="selected">{{ selected }}</span>
      <span v-else class="placeholder">Язык</span>
    </button>
    <div v-show="showingOptions" class="dropdown">
      <ul class="items">
        <li v-for="(item, index) in options" :key="index" class="item" @click="select(item)">
          {{ item }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    language: String,
    options: Array,
  },
  data() {
    return {
      selected: '',
      showingOptions: false,
    };
  },
  methods: {
    select(value) {
      this.selected = value;
      this.$emit('input', value);
      this.showingOptions = false;
    },
    showOptions() {
      this.showingOptions = !this.showingOptions;
    },
  },
};
</script>

<style lang="scss" scoped>
  .select {
    position: relative;

    .trigger {
      @include baseFormField;
      @include centerPositionFlex;
      cursor: pointer;

      .placeholder {
        color: $colorSecondaryText;
      }

      &.down {
        border: 2px solid $colorPrimary;
        padding: calc(#{$formFieldPadding} - 1px);
      }

      &::after {
        content: '';
        width: 1rem;
        height: 1rem;
        background-color: $colorPrimary;
        mask-image: url('../../assets/svg-icons/select-arrow.svg');
        @include iconMaskImage;
      }

      &.down::after {
        transform: rotate(180deg);
      }
    }

    .dropdown {
      position: absolute;
      top: 4rem;
      left: 0;
      right: 0;
      color: $colorSecondaryText;
      color: $colorTextActive;
      max-height: 12rem;
      overflow: scroll;
      background: $baseBackgroundColor;
      border: 1px solid $nonActiveColor;
      border-radius: $baseBorderRadius;
      box-shadow: $baseBoxShadow, 0px 20px 20px rgba(44, 39, 56, 0.04);

      .items {
        margin: 0;
        padding: 0;
        list-style: none;

        .item {
          padding: 0.8rem 1rem;

          &:hover {
            background-color: #EBF4F8;
            cursor: pointer;
          }
        }
      }
    }
  }
</style>
