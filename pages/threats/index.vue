<template>
  <section class="section has-background-light">
    <h1 class="title">Threat Overview</h1>
    <div class="columns">
      <div class="column is-one-third">
        <div class="card">
          <div class="card-content">
            <h2 class="title is-size-5">Most recent threat</h2>
            <div>{{ latest.name }}</div>
          </div>
          <div class="card-footer">
            <div class="card-footer-item">
              <b-button
                type="is-primary is-marginless"
                tag="nuxt-link"
                expanded
                :to="{ name: 'threats-id', params: { id: latest.id } }"
              >Open</b-button>
            </div>
          </div>
        </div>

      </div>
      <div class="column is-one-third">
        <div class="card">
          <div class="card-content">
            <h2 class="title is-size-5">Latest Critical threat</h2>
            <div>{{ critical.name }}</div>
          </div>
          <div class="card-footer">
            <div class="card-footer-item">
              <b-button
                type="is-primary is-marginless"
                tag="nuxt-link"
                expanded
                :to="{ name: 'threats-id', params: { id: critical.id } }"
              >Open</b-button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import Mock from '~/assets/mock.json';

export default {
  data: () => ({
    threats: Mock.threats,
  }),
  computed: {
    latest () {
      return this.threats[0];
    },
    critical () {
      return this.threats.find(t => t.severity === 'Critical');
    },
  },
};
</script>


<style lang="scss" scoped>
section {
  background: url("~assets/background.png") no-repeat bottom -220px right -350px;
  background-size: 1000px;
}
</style>
