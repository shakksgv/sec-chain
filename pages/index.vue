<template>
  <div>
    <div class="box is-flex is-paddingless is-radiusless is-marginless">
      <div class="media">
        <div class="media-left">
          <b-icon type="settings" />
        </div>
        <div class="media-content">
          <div>@Tier1</div>
          <small>Assign to me</small>
        </div>
        <div class="media-right">
          <b-icon type="caret" />
        </div>
      </div>

      <div class="media grow">
        <div class="media-content">
          <span class="is-size-5 has-text-weight-semibold">Account Creation</span>
          <span>ID: 12345</span>
        </div>
      </div>
    </div>
    <div class="section">
      <b-tabs>
        <b-tab-item label="Overview">
          <h4 class="title is-size-5">Alerts (4)</h4>
          <div class="columns">
            <div
              class="column"
              v-for="i in 3"
              :key="i"
            >
              <div class="card">
                <div class="card-content">
                  <b>Alert name</b>
                  <div>
                    <small>21.01.2019 11:00</small>
                  </div>
                  <br />
                  <div class="level">
                    <div>2 Events</div>
                    <b-icon icon="settings" />
                  </div>
                </div>
              </div>
            </div>
          </div>

          <apexchart
            type=scatter
            height=350
            :options="chartOptions"
            :series="series"
          />

          <h4 class="title is-size-5">Insights (5)</h4>
          <div class="columns">
            <div
              class="column"
              v-for="i in 4"
              :key="i"
            >
              <div class="card">
                <div class="card-header">
                  <div class="card-header-title">Title</div>
                </div>
                <div class="card-content">
                  <div>
                    <b-icon icon="account" />
                    Alert name
                  </div>
                  <br />
                  <div>
                    <div>Name: John Wick</div>
                    <div>Role: The Man</div>
                    <div>The: Legend</div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </b-tab-item>
        <b-tab-item label="Case Wall"></b-tab-item>
        <b-tab-item label="Events"></b-tab-item>
      </b-tabs>
    </div>
  </div>
</template>

<script>
function generateDayWiseTimeSeries (baseval, count, yrange) {
  var i = 0;
  var series = [];
  while (i < count) {
    var y = Math.floor(Math.random() * (yrange.max - yrange.min + 1)) + yrange.min;

    series.push([baseval, y]);
    baseval += 86400000;
    i++;
  }
  return series;
}

export default {
  data: () => ({
    series: [{
      name: 'Low risk',
      data: generateDayWiseTimeSeries(new Date('11 Nov 2019 GMT').getTime(), 20, {
        min: 10,
        max: 30
      })
    },
    {
      name: 'Medium risk',
      data: generateDayWiseTimeSeries(new Date('11 Nov 2019 GMT').getTime(), 15, {
        min: 31,
        max: 70
      })
    },
    {
      name: 'High risk',
      data: generateDayWiseTimeSeries(new Date('11 Nov 2019 GMT').getTime(), 20, {
        min: 71,
        max: 100
      })
    },
    ],
    chartOptions: {
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
        max: 100
      }
    }
  }),
}
</script>

<style lang="scss" scoped>
.media {
  margin: 0;
  padding: 0.5rem;

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
