<template>
  <div
    id="container"
    class="is-flex"
  >
    <Sidebar />
    <nuxt-child
      id="page"
      :key="$route.params.id"
    />
  </div>
</template>

<script>
import Sidebar from '~/components/sidebar';

export default {
  components: { Sidebar },
}
</script>

<style lang="scss" scoped>
#container {
  flex-grow: 1;
}

#page {
  flex-grow: 1;
}
</style>
