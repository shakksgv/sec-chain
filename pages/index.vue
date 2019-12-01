<template>
  <section class="section has-background-light">
    <div class="columns">
      <div class="column is-half">
        <div class="columns is-multiline">
          <div class="column">
            <div class="box">
              <h3 class="title">Today</h3>
              <div>Total threats:
                <animated-number
                  :value="newToday"
                  round=1
                />
              </div>
              <div>Open threats:
                <animated-number
                  :value="openToday"
                  round=1
                />
              </div>
            </div>
          </div>
          <div class="column">
            <div class="box">
              <h3 class="title">All time</h3>
              <div>Total threats:
                <animated-number
                  :value="total"
                  round=1
                />
              </div>
              <div>Open threats:
                <animated-number
                  :value="open"
                  round=1
                />
              </div>
            </div>
          </div>

          <div class="column is-full">
            <div class="box">
              <div class="level">
                <h2 class="title is-marginless">Latest threats</h2>
                <nuxt-link to="/threats">See all</nuxt-link>
              </div>
              <b-table
                :data="threats.slice(0, 5)"
                hoverable
                striped
              >
                <template v-slot="{ row }">
                  <b-table-column
                    label="Severity"
                    sortable
                    width="100"
                    field="severity"
                  >
                    <b-tag :type="severityType(row.severity)">{{ row.severity }}</b-tag>
                  </b-table-column>
                  <b-table-column
                    label="Name"
                    field="name"
                    sortable
                  >
                    <nuxt-link
                      class="has-text-dark"
                      :to="{ name: 'threats-id', params: { id: row.id } }"
                    >
                      <u>{{ row.name }}</u>
                    </nuxt-link>
                  </b-table-column>
                  <b-table-column
                    label="Status"
                    field="status"
                    sortable
                  >
                    <b-tag :type="row.status === 'Open' ? 'is-primary' : ''">{{ row.status }}</b-tag>
                  </b-table-column>
                  <b-table-column
                    label="Resolution"
                    field="resolution"
                    sortable
                  >{{ row.resolution || 'None' }}</b-table-column>
                </template>
              </b-table>
            </div>
          </div>
        </div>
      </div>

      <div class="column">
        <div class="columns is-multiline">
          <div class="column is-full">
            <div class="box">
              <div class="level">
                <apexchart
                  type=radialBar
                  height=280
                  :options="{ labels: ['Resolved'], ...radialOptions }"
                  :series="[closed / total * 100]"
                />
                <apexchart
                  type=radialBar
                  height=280
                  :options="{ labels: ['False Positive'], ...radialOptions }"
                  :series="[falsePositive / total * 100]"
                />
              </div>
            </div>
          </div>
          <div class="column">
            <div class="box">

              <apexchart
                type=area
                height=350
                :options="chartOptions"
                :series="series"
              />
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import Mock from '~/assets/mock.json';
import AnimatedNumber from "animated-number-vue";

const datesAreOnSameDay = (first, second) =>
  first.getFullYear() === second.getFullYear() &&
  first.getMonth() === second.getMonth() &&
  first.getDate() === second.getDate();

export default {
  components: { AnimatedNumber },
  data: () => ({
    threats: Mock.threats,
    chartOptions: {
      chart: {
        stacked: false,
        zoom: {
          type: 'x',
          enabled: true,
          autoScaleYaxis: true
        },
        toolbar: {
          autoSelected: 'zoom'
        }
      },
      dataLabels: {
        enabled: false
      },
      markers: {
        size: 0,
      },
      fill: {
        type: 'gradient',
        colors: '#2f1c63',
        gradient: {
          shadeIntensity: 1,
          inverseColors: false,
          opacityFrom: 0.5,
          opacityTo: 0,
          stops: [0, 90, 100]
        },
      },
      stroke: {
        colors: ['#2f1c63'],
      },
      xaxis: {
        type: 'datetime',
      },

      yaxis: {
        min: 0,
      },

      tooltip: {
        shared: false,
        y: {
          formatter: function (val) {
            return val.toFixed(0)
          }
        }
      }
    },

    radialOptions: {
      colors: ["#20E647"],
      plotOptions: {
        radialBar: {
          hollow: {
            margin: 0,
            size: "70%",
            background: "#293450"
          },
          track: {
            dropShadow: {
              enabled: true,
              top: 2,
              left: 0,
              blur: 4,
              opacity: 0.15
            }
          },
          dataLabels: {
            name: {
              offsetY: -10,
              color: "#fff",
              fontSize: "13px"
            },
            value: {
              color: "#fff",
              fontSize: "30px",
              show: true
            }
          }
        }
      },
      fill: {
        type: "gradient",
        gradient: {
          shade: "dark",
          type: "vertical",
          gradientToColors: ["#87D4F9"],
          stops: [0, 100]
        }
      },
      stroke: {
        lineCap: "round"
      },
    },
  }),
  computed: {
    total () {
      return this.threats.length;
    },
    open () {
      return this.threats.reduce((acc, i) => {
        return i.status === 'Closed' ? acc : acc + 1;
      }, 0);
    },
    closed () {
      return this.total - this.open;
    },
    falsePositive () {
      return this.threats.reduce((acc, i) => {
        return i.resolution === 'F/P' ? acc + 1 : acc;
      }, 0);
    },

    newToday () {
      return this.threats.reduce((acc, i) => {
        return datesAreOnSameDay(new Date(i.date), new Date()) ? acc + 1 : acc;
      }, 0);
    },

    openToday () {
      return this.threats.reduce((acc, i) => {
        return datesAreOnSameDay(new Date(i.date), new Date()) && i.status === 'Open' ? acc + 1 : acc;
      }, 0);
    },

    series () {
      const data = [];
      this.threats.forEach((t, i) => {
        const date = new Date(t.date);
        const record = data.find(i => i[0].getTime() === date.getTime());

        if (record) {
          record[1] += 1;
        } else {
          data.push([new Date(t.date), 1]);
        }
      });

      return [{
        name: 'Threats',
        data,
      }];
    },
  },
  methods: {
    severityType (severity) {
      switch (severity) {
        case 'Medium':
          return 'is-warning';
        case 'High':
        case 'Critical':
          return 'is-danger';
        default:
          return 'is-grey';
      }
    },
  },
}
</script>

<style lang="scss" scoped>
section {
  background: url(/_nuxt/assets/background.png) no-repeat bottom -220px right -350px;
  background-size: 1000px;
}
</style>
