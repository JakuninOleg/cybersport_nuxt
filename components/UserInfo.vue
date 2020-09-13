<template>
  <div class="user-data">
    <div class="user-data__controls">
      <button :class="['button', 'button_chart-toggle']" @click="changeDataView">
        Show {{ dataView === 'table' ? 'chart' : 'table' }}
      </button>
      <template v-if="dataView == 'table'">
        <Slicer
          v-for="title in titles"
          :key="'slicer-' + title"
          :title="title"
          :class="['button', shownColumns[title] === false ? 'button_pushed' : 'button_orange']"
          @toggleColumn="toggleColumn"
        />
        <span>
          Mouse dx: {{ mouseDx }}
          (filter results > value)
        </span>
        <vue-range
          v-model="mouseDx"
          name="mouseDx"
          class="slider"
          :min="-2"
          :max="2"
          :step="1"
          :marks="marks"
          :adsorb="true"
          @change="filterData"
        />
      </template>
    </div>
    <template v-if="dataView == 'table'">
      <div class="user-data__table">
        <template v-for="(array, title) in userData">
          <TableColumn
            v-if="shownColumns[title]"
            :key="'tr-' + title + userData.time.length"
            :title="title"
            :data-array="array"
          />
        </template>
      </div>
    </template>
    <template v-else>
      <Chart :chart-data="chartData" />
    </template>
  </div>
</template>
<script>
import TableColumn from '@/components/TableColumn'
import Slicer from '@/components/Slicer'
import Chart from '@/components/Chart'

export default {
  components: {
    TableColumn,
    Slicer,
    Chart
  },
  props: {
    userJson: {
      required: true,
      type: Array
    }
  },
  data () {
    return {
      userData: {},
      dataView: 'table',
      mouseDx: 0.0,
      marks: [-2, 0, 2],
      shownColumns: null,
      chartData: []
    }
  },
  computed: {
    titles () {
      return Object.keys(this.userData)
    }
  },
  created () {
    this.transformJson(this.userJson)
    this.createColumnsObject()
    this.generateChartData()
  },
  methods: {
    toggleColumn (title) {
      this.shownColumns[title] = !this.shownColumns[title]
    },
    changeDataView () {
      if (this.dataView === 'table') {
        this.dataView = 'chart'
      } else {
        this.dataView = 'table'
      }
    },
    createColumnsObject () {
      const columns = {}
      this.titles.forEach((title) => {
        columns[title] = true
      })
      this.shownColumns = columns
    },
    transformJson (json) {
      const newObj = {}
      for (const el in Object.keys(json[0])) {
        const title = Object.keys(json[0])[el]
        const csvObj = { [title]: json.map(el => el[title]) }
        Object.assign(newObj, csvObj)
      }
      this.userData = newObj
    },
    generateChartData () {
      this.chartData = []
      this.chartData.push(['Time', 'Mouse_dx', 'Mouse_dy'])
      for (let i = 0; i < 10; i++) {
        this.chartData.push([new Date(this.userData.time[i]), this.userData.mouse_dx[i], this.userData.mouse_dy[i]])
      }
    },
    filterData () {
      const filteredJson = this.userJson.filter(el => el.mouse_dx > this.mouseDx)
      this.transformJson(filteredJson)
      this.generateChartData()
    }
  }
}
</script>

<style lang="scss">
.user-data {
  display: grid;
  grid-template-columns: 100px 1fr;
  gap: 50px;

  &__controls {
    display: grid;
    gap: 12px;
    align-self: flex-start;
  }

  &__table {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(100px, max-content));
    word-wrap: break-word;
  }
}
</style>
