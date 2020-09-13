<template>
  <div class="container">
    <div class="header">
      <h1 class="heading-1">
        Welcome, Champion!
      </h1>
      <h2 class="heading-1">
        Choose data to examine your stats
      </h2>
    </div>
    <div class="data-buttons">
      <button
        v-for="id in 10"
        :key="'button-id-' + id"
        :disabled="id == dataId"
        :class="['button', id == dataId ? 'button_pushed' : 'button_blue']"
        @click="changeDataSet(id)"
      >
        DataSet: {{ id }}
      </button>
    </div>
    <UserView :key="'data' + dataId" :data-id="dataId" />
  </div>
</template>

<script>
import UserView from '@/components/UserView'

export default {
  components: {
    UserView
  },
  data () {
    return {
      dataId: 1
    }
  },
  methods: {
    changeDataSet (id) {
      this.dataId = id
    }
  }
}
</script>

<style lang="scss">
*,
*::before,
*::after {
  margin: 0;
  padding: 0;
  box-sizing: inherit;
}

html {
  box-sizing: border-box;
  scroll-behavior: smooth;
}

body {
  overflow-y:scroll;
}

.container {
  display: grid;
  width: 70%;
  margin: 0 auto;
}

.header {
  margin: 20px 0;
}

.data-buttons {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(10px, 1fr));
  margin: 25px 0;
}

.heading-1, heading-2 {
  text-align: center;
}

.button {
  border: none;
  padding: 6px 5px;
  transition: all 0.32s ease-out;
  font-size: 16px;
  color: white;

  &:focus {
    outline: none;
  }

  &:hover {
    cursor: pointer;
  }

  &:disabled {
    cursor: not-allowed;
  }

  &_pushed {
    background: #A0AEC0;
  }

  &_blue {
    background: #4299E1;

    &:hover {
      background: #3182CE;
    }
  }

  &_orange {
    background: #F6AD55;
    &:hover {
      background: #ED8936;
    }
  }

  &_chart-toggle {
    background: #FFFFFF;
    color: black;
    border: 1px solid black;
    font-weight: 500;

    &:hover {
      background: black;
      color: white;
    }
  }
}
</style>
