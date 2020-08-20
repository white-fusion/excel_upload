<template>
  <div id="app">
    <UploadButton @onSubmit="submit" v-bind:dataStruct="dataStruct" v-bind:titleText="titleText"/>
  </div>
</template>

<script>
import UploadButton from './components/UploadButton.vue'

export default {
  name: 'App',
  methods:{
    submit(obj){
      console.log(obj);
    }
  },
  data() {
    return {
      titleText: "Upload File",
      dataStruct: {
        startDate: {
          meta:{
            hasSubfields: false,
            type: ["date"],
            mandatory: false
          }
        },
        endDate: {
          meta: {
            hasSubfields: false,
            type: ["date"],
            mandatory: true
          }
        },
        estimatedTime: {
          meta: {
            hasSubfields: false,
            type: ["number"],
            mandatory: false,
            default: 0
          }
        },
        estimate: {
          meta: {
            hasSubfields: true
          },
          fields: {
            estimateLabor: {
              meta: {
                hasSubfields: false,
                type: ["number"],
                mandatory: false,
                //default: 0
              }
            },
            estimateEquipment: {
              meta: {
                hasSubfields: false,
                type: ["number"],
                mandatory: false,
                default: 0
              }
            },
            estimateMaterial: {
              meta: {
                hasSubfields: false,
                type: ["number"],
                mandatory: false,
                default: 0
              }
            },
            estimateOthers: {
              meta: {
                hasSubfields: false,
                type: ["number"],
                mandatory: false,
                default: 0
              }
            },
            estimateSub: {
              meta: {
                hasSubfields: false,
                type: ["number"],
                mandatory: false,
                default: 0
              }
            },
            estimateBidValue: {
              meta: {
                hasSubfields: false,
                type: ["number"],
                mandatory: false,
                default: 0
              }
            }
          }
        },
        orderSequence: {
          meta: {
            hasSubfields: false,
            type: ["string"],
            mandatory: false,
            default: ''
          }
        },
        taskCode: {
          meta: {
            hasSubfields: false,
            type: ["number"],
            mandatory: true
          }
        },
        title: {
          meta: {
            hasSubfields: false,
            type: ["string"],
            mandatory: false
          }
        },
        type: {
          meta: {
            hasSubfields: false,
            mandatory: false,
            default: 'F'
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
