<template>
  <div id="menu">
    <nuxt-link
      v-for="(threat) in threats"
      :to="{ name: 'threats-id', params: { id: threat.id } }"
      :key="threat.id"
      :style="{ opacity: threat.resolution === 'F/P' ? 0.6 : 1 }"
      class="media"
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
#menu {
  animation: slide-in 0.3s ease-in forwards;
  background: #170b38;
  box-shadow: 0 0 10px grey;
  flex-shrink: 0;
  position: relative;
  width: 350px;
  z-index: 1;
}

.media {
  color: white;
  margin: 0;
  padding: 1rem;
  position: relative;
  transition: all 0.3s;

  &:hover:after {
    transform: scaleX(1);
  }

  &:after {
    background: white;
    bottom: 0;
    content: "";
    left: 0;
    opacity: 0.05;
    pointer-events: none;
    position: absolute;
    right: 0;
    top: 0;
    transform: scaleX(0);
    transform-origin: left;
    transition: all 0.3s;
    z-index: 1;
  }

  &.nuxt-link-active {
    background: #0c051f;
  }
}

@keyframes slide-in {
  0% {
    transform: translateX(-100%);
  }
  100% {
    transform: translateX(0);
  }
}
</style>
