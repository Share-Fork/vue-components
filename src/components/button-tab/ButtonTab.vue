<template>
  <div :class="_clas" :style="styles" onselectstart="return false;">
    <slot></slot>
  </div>
</template>

<script>
import { cssPrefix } from 'utils/variable.js'
import { base } from 'utils/mixins.js'
export default {
  mixins: [base, {
    mounted () {
      if (!this.$children) return
      this.childLength = this.$children.length
      this.$children.forEach((item, i) => {
        this.$children[i].$on('on-change', this.changeHandler)
        item.index = i
        item.active = item.index === this.active
      })
    },
    props: {
      active: {
        type: Number,
        default: 0
      }
    },
    watch: {
      active (val, oldVal) {
        this.$children[oldVal].active = false
        this.$children[val].active = true
      }
    }
  }],
  computed: {
    _clas () {
      return [cssPrefix + 'button-tab', this.clas]
    }
  },
  data () {
    return {
      cssPrefix: cssPrefix,
      childLength: this.$children.length
    }
  },
  methods: {
    changeHandler (val) {
      this.$emit('click', val)
      if (val !== this.active) {
        this.$emit('on-change', val)
      }
    }
  }
}
</script>

<style lang="scss">
  @import '~styles/variable.scss';
  .#{$css-prefix}{
    &button-tab{
      display:flex;
      text-align:center;
      position:relative;
      border:1px solid $primary-color;
      border-radius:48px;
      overflow:hidden;
      user-select: none;
    }
  }
</style>
