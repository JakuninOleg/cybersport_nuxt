<template>
  <div class="table-column">
    <div class="table-header table-row">
      {{ title }}
    </div>
    <div v-for="(data, id) in localArray" :key="'data-row' + id" class="table-row">
      {{ data }}
    </div>
  </div>
</template>

<script>
import { format } from 'date-fns'
import { ru } from 'date-fns/locale'

export default {
  props: {
    title: {
      type: String,
      required: true
    },
    dataArray: {
      type: Array,
      required: true
    }
  },
  data () {
    return {
      localArray: this.dataArray
    }
  },
  created () {
    if (this.title === 'time') {
      this.localArray = this.localArray.map(element => format(new Date(element), 'do MMM yyyy hh:mm:ss', { locale: ru }))
    }
  }
}
</script>

<style lang="scss">
  .table-row {
    background: #F7FAFC;
    padding: 2px 5px;

    &:nth-child(odd) {
      background: #EDF2F7;
    }
  }
</style>
