<template>
  <layout>
    <x-header :back="false" slot="header">
      <div slot="title" style="text-align:center">
        Demos
      </div>
    </x-header>
    <x-body class="demos" slot="body" style="background:#fff">
      <div class="demos-actions">
        <x-link href="/button">XButton (完成)</x-link>
        <x-link href="/input">XInput (完成)</x-link>
        <x-link href="/input">Password (完成)</x-link>
        <x-link href="/textarea">XTextarea (完成)</x-link>
        <x-link href="/select">XSelect</x-link>
        <x-link href="/switch">XSwitch</x-link>
        <x-link href="/radio">Radio</x-link>
        <x-link href="/checkbox">Checkbox</x-link>
        <x-link href="/range">Range</x-link>
        <x-link href="/search">Search</x-link>
        <x-link href="/actionsheet">Actionsheet</x-link>
        <x-link href="/alert">Alert</x-link>
        <x-link href="/confirm">Confirm</x-link>
        <x-link href="/tab">Tab</x-link>
        <x-link href="/tabbutton">TabButton</x-link>
        <x-link href="/tabbar">Tabbar</x-link>
        <x-link href="/popup">Popup</x-link>
        <x-link href="/toast">Toast</x-link>
        <x-link href="/flow">Flow</x-link>
        <x-link href="/ripple">Ripple</x-link>
        <x-link href="/img">XImg</x-link>
        <x-link href="/header">XHeader</x-link>
        <x-link href="/divider">Divider</x-link>
        <x-link href="/swiper">Swiper</x-link>
        <x-link href="/preview">Preview</x-link>
      </div>
    </x-body>
  </layout>
</template>
<script>
import {
  XHeader,
  Layout,
  XBody,
  XLink
} from '@/components'

import 'normalize.css'
export default {
  components: {
    XHeader,
    Layout,
    XBody,
    XLink
  }
}

</script>
<style lang="less">
.demos {
  h1 {
    text-align: center;
    margin: 0;
    padding: 1rem 0;
  }
}

.demos-actions {
  display: flex;
  flex-wrap: wrap;
  text-align: center;
  a {
    display: block;
    width: 33.33%;
    line-height: 44px;
  }
}

.input-demo {
  padding-top: 20px;
}

body{
  padding:0;
  margin:0;
  background:#f7f7f7;
  overflow:hidden;
}
</style>

