<template>
  <div id="layout">
    <Header />
    <nuxt id="content" />
  </div>
</template>

<script>
import Header from '~/components/header';

export default {
  components: { Header },
}
</script>

<style scoped>
#layout {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

#content {
  flex-grow: 1;
}
</style>
