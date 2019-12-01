<template>
  <div id="layout">
    <b-navbar type="is-primary">
      <template slot="brand">
        <b-navbar-item
          tag="nuxt-link"
          class="is-size-4"
          :active="false"
          to="/"
        >
          SecChain
        </b-navbar-item>
      </template>

      <template slot="start">
        <b-navbar-item
          tag="nuxt-link"
          :active="$route.name === 'index'"
          to="/"
        >Dashboard</b-navbar-item>
        <b-navbar-item
          tag="nuxt-link"
          :active="$route.name.indexOf('threats') === 0"
          to="/threats"
        >Threats</b-navbar-item>
      </template>

      <template slot="end">
        <b-navbar-item>
          <b-icon icon="bell" />
        </b-navbar-item>
        <b-navbar-item>
          <b-icon icon="logout" />
        </b-navbar-item>
      </template>
    </b-navbar>
    <nuxt id="content" />
  </div>

</template>

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
