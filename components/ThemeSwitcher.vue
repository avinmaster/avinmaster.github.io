<template lang="pug">
  button(@click="$emit('toggle')" :class="{ 'light': !isDark }").theme-switcher
    span(v-if="isDark").dark 🌞
    span(v-else).light 🌙
</template>

<script>
export default {
  name: 'ThemeSwitcher',
  props: {
    isDark: {
      type: Boolean,
      default: false
    }
  }
}
</script>

<style scoped lang="scss">
.theme-switcher {
  position: absolute;
  left: 4%;
  top: 0;
  z-index: 9999;
  padding: 10px;
  background-color: #f2f3f5;
  border: none;
  border-bottom-left-radius: 5px;
  border-bottom-right-radius: 5px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  color: #FFFFFF;
  font-size: 20px;
  transition: all .3s;

  &:hover {
    background: #ffffff;
  }

  &.light {
    background: #3b3b3b;

    &:hover {
      background: #2b2b2b;
    }
  }
}

.theme-switcher span {
  cursor: pointer;
  padding-right: 3px;
}
</style>
