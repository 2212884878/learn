<template>
  <div :class="_clas" @click="clickHandler">
    <slot></slot>
  </div>
</template>

<script>
import { cssPrefix } from 'utils/variable.js'
export default {
  methods: {
    clickHandler () {
      this.$emit('on-change', this.index)
    }
  },
  computed: {
    _clas () {
      return [cssPrefix + 'button-tab-item', this.active ? cssPrefix + 'button-tab-item-active' : '']
    }
  },
  data () {
    return {
      cssPrefix: cssPrefix,
      active: false
    }
  }
}
</script>

<style lang="scss">
  @import '~styles/variable.scss';
  @import '~styles/mixins.scss';
  .#{$css-prefix}{
    &button-tab-item{
      flex:1;
      border-left:1px solid $primary-color;
      padding:0.55rem 0;
      &-active{
        background-color:$primary-color;
        color:#fff;
        transition: all $transition-time $ease-in-out;
      }
      &:first-child{
        border-left:0;
      }
    }
  }
</style>
