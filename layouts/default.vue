<template>
  <div id="layout">
    <b-navbar type="is-primary">
      <template slot="end">
        <b-navbar-item tag="div">
          <div class="buttons">
            <a class="button is-primary">
              <strong>Sign up</strong>
            </a>
            <a class="button is-light">
              Log in
            </a>
          </div>
        </b-navbar-item>
      </template>
    </b-navbar>

    <div
      id="container"
      class="is-flex"
    >
      <div id="menu">
        <div
          v-for="i in 5"
          :key="i"
          class="media"
        >
          <div class="media-left">
            <b-icon icon="account" />
          </div>
          <div class="media-content">
            <div>Threat name</div>
            <small class="has-text-grey">21.01.2019</small>
          </div>
          <div class="media-right">
            <b-icon icon="settings" />
            <b-icon icon="earth" />
            <b-icon icon="tune" />
          </div>
        </div>
      </div>
      <nuxt id="page" />
    </div>
  </div>

</template>

<style scoped>
#layout {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

#container {
  flex-grow: 1;
}

#menu {
  box-shadow: 0 0 10px grey;
  padding: 1rem 0.5rem;
  position: relative;
  width: 350px;
  z-index: 1;
}

#page {
  flex-grow: 1;
}
</style>
