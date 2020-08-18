<template>
  <div id="app">
    <UploadButton v-bind:dataStruct="dataStruct"/>
  </div>
</template>

<script>
import UploadButton from './components/UploadButton.vue'

export default {
  name: 'App',
  data() {
    return {
      dataStruct: {
        startDate: {
          meta:{
            column: true,
            type: [Date],
            mandatory: true
          }
        },
        xyz:{
          meta:{
            column: true,
            type: [Number],
            mandatory: false
          }
        },
        estimates: {
          meta: {
            column: false
          },
          fields: {
            subcontractor: {
              meta: {
              column: true,
              type: [String, Number],
              mandatory: false
              }
            },
            equipment: {
              meta: {
                column: true,
                type: [String, Number],
                mandatory: true
              }
            },
            abc: {
              meta: {
                column: true,
                type: [String],
                mandatory: false
              }
            }
          }  
        }
      }
    }
  },
  components: {
    UploadButton
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
