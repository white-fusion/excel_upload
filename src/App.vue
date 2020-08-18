<template>
  <div id="app">
    <UploadButton v-bind:dataStruct="dataStruct" v-bind:titleText="titleText"/>
  </div>
</template>

<script>
import UploadButton from './components/UploadButton.vue'

export default {
  name: 'App',
  data() {
    return {
      titleText: "Upload File",
      dataStruct: {
        startDate: {
          meta:{
            column: true,
            type: ["date"],
            mandatory: true
          }
        },
        xyz:{
          meta:{
            column: true,
            type: ["number"],
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
              type: ["string", "number"],
              mandatory: false
              }
            },
            equipment: {
              meta: {
                column: true,
                type: ["string", "number"],
                mandatory: true
              }
            },
            abc: {
              meta: {
                column: false,
              },
              fields: {
                SNo: {
                  meta:{
                    column: true,
                    type: ["string", "number"],
                    mandatory: true
                  }
                }
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
