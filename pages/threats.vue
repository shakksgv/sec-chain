<template>
  <div
    id="container"
    class="is-flex"
  >
    <div id="menu">
      <nuxt-link
        v-for="(threat, index) in threats"
        :to="{ name: 'threats-id', params: { id: index } }"
        :key="index"
        :style="{ opacity: threat.resolution === 'F/P' ? 0.6 : 1 }"
        class="media has-text-dark"
      >
        <div class="media-left">
          <b-icon
            icon="alert"
            :class="{
                'has-text-grey': threat.severity === 'Info',
                'has-text-grey': threat.severity === 'Low',
                'has-text-warning': threat.severity === 'Medium',
                'has-text-danger': threat.severity === 'High',
                'has-text-danger': threat.severity === 'Critical',
            }"
          />
        </div>
        <div class="media-content">
          <div class="name">{{ threat.name }}</div>
          <small class="has-text-grey">
            <span>Resolution:</span>
            {{ threat.resolution || 'None' }}
          </small>
        </div>
        <div class="media-right">
          <b-tooltip
            :label="threat.assigned_analist"
            position="is-bottom"
          >
            <b-icon icon="account" />
          </b-tooltip>
        </div>
      </nuxt-link>
    </div>
    <nuxt-child id="page" />
  </div>
</template>

<script>
import Mock from '~/assets/mock.json';

export default {
  data: () => ({
    threats: Mock.threats,
  }),
}
</script>

<style lang="scss" scoped>
#container {
  flex-grow: 1;
}

#menu {
  box-shadow: 0 0 10px grey;
  flex-shrink: 0;
  padding: 1rem 0.5rem;
  position: relative;
  width: 350px;
  z-index: 1;
}

#page {
  flex-grow: 1;
}

.media.nuxt-link-active {
  .name {
    font-weight: bold;
  }
}
</style>
