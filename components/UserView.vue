<template>
  <div>
    <template v-if="!error">
      <template v-if="loading">
        <Loading />
      </template>
      <UserInfo v-else :user-json="csv" />
    </template>
    <template v-else>
      Возникла ошибка:{{ error }}
    </template>
  </div>
</template>

<script>
import UserInfo from '@/components/UserInfo'
import Loading from '@/components/Loading'

export default {
  components: {
    UserInfo,
    Loading
  },
  props: {
    dataId: {
      required: true,
      type: Number
    }
  },
  data () {
    return {
      csv: {},
      loading: true,
      error: null
    }
  },
  async created () {
    try {
      const json = await this.$axios.get(
        `https://fastapi-cs.herokuapp.com/api/data/${this.dataId}`
      )
      this.loading = false
      this.csv = json.data
    } catch (err) {
      this.error = err.message
    }
  }
}
</script>
