<script>
import DimiLoader from './DimiLoader.vue'

export default {
  name: 'DimiButton',

  components: { DimiLoader },

  props: {
    gray: {
      type: Boolean,
      default: false
    },
    active: {
      type: Boolean,
      default: true
    },
    loading: {
      type: Boolean,
      default: false
    },
    href: {
      type: String,
      default: undefined
    },
    small: {
      type: Boolean,
      default: false
    },
    large: {
      type: Boolean,
      default: false
    },
    text: {
      type: Boolean,
      default: false
    }
  },

  computed: {
    computedClass () {
      return {
        'btn': true,
        'btn--gray': this.gray,
        'btn--small': this.small,
        'btn--large': this.large,
        'btn--text': this.text,
        'btn--cursor-disable': !this.isActive
      }
    },

    isActive () {
      return !this.loading && this.active
    }
  },

  methods: {
    click (e) {
      if (this.active) this.$emit('click', e)
    }
  }
}
</script>

<template>
  <a
    v-ripple="'rgba(255, 255, 255, .2)'"
    :class="computedClass"
    :href="href"
    @click="click"
  >
    <slot/>
  </a>
</template>

<style lang='scss'>
@import '../scss/vars';

.btn {
  align-items: center;
  appearance: none;
  background-color: $red;
  border-radius: 2rem;
  color: $white;
  cursor: pointer;
  display: inline-flex;
  font-weight: $font-weight-bold;
  justify-content: center;
  overflow: hidden;
  padding: 0.8em 2.7em;
  position: relative;
  text-decoration: none;
  transition: 0.5s background-color ease;
  user-select: none;
  white-space: nowrap;

  &--gray {
    background-color: $gray-lighten;
    color: $gray-dark;
  }

  &--text {
    background-color: transparent;
  }

  &--cursor-disable {
    cursor: not-allowed;
  }

  &--large {
    font-size: 24px;
  }

  &--small {
    font-size: 12px;
  }
}

</style>
