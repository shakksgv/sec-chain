<template>
  <div class="has-background-light">
    <div class="box is-flex is-paddingless is-radiusless is-marginless">
      <div class="media">
        <div class="media-content">
          <span class="is-size-5 has-text-weight-semibold">{{ threat.name }}</span>
          <div>
            <b-icon
              icon="clock"
              size="is-small"
            />
            {{ threat.date }}
          </div>
          <div class="level-left">Resolution: &nbsp;
            <b-select
              size="is-small"
              v-model="threat.resolution"
            >
              <option value="">None</option>
              <option value="F/P">F/P</option>
              <option value="Positive">Positive</option>
            </b-select>
          </div>
        </div>
      </div>

      <div class="media grow">
        <div class="media-content">
          <div class="level-left">
            <b-icon
              icon="alert"
              size="is-small"
            />
            &nbsp; Severity: &nbsp;
            <b-select
              size="is-small"
              v-model="threat.severity"
            >
              <option value="Info">Info</option>
              <option value="Low">Low</option>
              <option value="Medium">Medium</option>
              <option value="High">High</option>
              <option value="Critical">Critical</option>
            </b-select>
          </div>
          <div>
            <b-icon
              icon="account"
              size="is-small"
            />
            Assignee: {{ threat.assigned_analist }}</div>
          <small v-if="threat.details.comments.length">
            <b-icon
              icon="comment"
              size="is-small"
            />
            Comment:
            <span class="has-text-grey">
              {{ threat.details.comments[0].text }}
            </span>
          </small>
        </div>
        <div class="media-right">
          <b-icon type="caret" />
        </div>
      </div>

      <div class="media grow">
        <div class="media-content">
          <div>
            <b-icon
              icon="account"
              size="is-small"
            />
            Owner: {{ threat.details.entity.owner }}</div>
          <div>
            <b-icon
              icon="laptop"
              size="is-small"
            />
            Device: {{ threat.details.entity.computer_name }}</div>
          <div>
            <small>
              <b-icon
                icon="earth"
                size="is-small"
              />
              IP: {{ threat.details.entity.ip }}</small>
          </div>
        </div>
      </div>
    </div>
    <div class="section">
      <b-tabs>
        <b-tab-item label="Overview">
          <h4 class="title is-size-5">Alerts ({{ threat.details.alerts.length }})</h4>
          <div class="columns is-multiline">
            <div
              class="column is-one-third"
              v-for="(alert, i) in threat.details.alerts"
              :key="i"
            >
              <div class="card">
                <div class="card-header has-background-primary">
                  <p class="card-header-title has-text-white">
                    {{ alert.source }}
                  </p>
                </div>
                <div class="card-content">
                  <b>{{ alert.name }}</b>
                  <div>
                    {{ alert.description }}
                  </div>
                  <template v-if="alert['Malicious_detections']">
                    <div v-if="alert.text">
                      <small>{{ alert.text }}</small>
                    </div>
                    <ul v-if="alert['Malicious_detections'].length">
                      <li
                        v-for="(d, i) in alert['Malicious_detections']"
                        :key="i"
                      >
                        <small>- {{ d }}</small>
                      </li>
                    </ul>
                  </template>

                  <div v-if="alert.signature">
                    <small>Signature: {{ alert.signature }}</small>
                  </div>

                  <br />
                  <div>{{ alert.number_event || 0 }} event(-s)</div>
                  <div class="level">
                    <small>{{ alert.time }}</small>
                    <small v-if="alert.dest_host">
                      <b-icon
                        icon="earth"
                        size="is-small"
                      />
                      Destination: &nbsp;
                      {{ alert.dest_host }}
                    </small>
                  </div>
                </div>
              </div>
            </div>
          </div>

          <apexchart
            type=scatter
            height=300
            :options="chartOptions"
            :series="series"
          />
        </b-tab-item>
      </b-tabs>
    </div>
  </div>
</template>

<script>
import Mock from '~/assets/mock.json';

export default {
  data: () => ({
    chartOptions: {
      colors: ['#f00'],
      chart: {
        background: '#fff'
      },
      dataLabels: {
        enabled: false
      },
      grid: {
        xaxis: {
          showLines: true
        },
        yaxis: {
          showLines: true
        },
      },
      xaxis: {
        type: 'datetime',

      },
      yaxis: {
        max: 10
      }
    }
  }),
  computed: {
    threat () {
      return Mock.threats.find(threat => this.$route.params.id == threat.id);
    },
    series () {
      return [
        {
          name: 'Alerts',
          data: this.threat.details.alerts.map((i, index) => [
            new Date(i.time), index + 1
          ]),
        },
      ];
    }
  },
}
</script>

<style lang="scss" scoped>
.media {
  flex: 1 0 auto;
  margin: 0;
  padding: 0.5rem;
  padding-left: 2rem;

  &.grow {
    border-left: 1px solid lightgrey;
  }
}

.section {
  padding: 1.5em;
}

.card-content {
  padding: 0.75rem;
}
</style>
