<template>
  <NuxtLink :to="{ path: '/' }" class="logo">RWC</NuxtLink>
</template>

<style lang="scss">
.logo {
  @include caption-h3;

  color: $var-color-main-brand;
  text-decoration: none;

  &:hover {
    color: $var-color-default;
  }
}
</style>
