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
      titleText: "UPLOAD",
      dataStruct: {
        companyName: {
          meta: {
            hasSubfields: false,
            type: ['string'],
            mandatory: true
          }
        },
        businessType: {
          meta: {
            hasSubfields: false,
            type: ['string'],
            mandatory: false
          }
        },
        contactName: {
          meta: {
            hasSubfields: false,
            type: ['string'],
            mandatory: false
          }
        },
        title: {
          meta: {
            hasSubfields: false,
            type: ['string'],
            mandatory: false
          }
        },
        email: {
          meta: {
            hasSubfields: false,
            type: ['string'],
            mandatory: false
          }
        },
        country: {
          meta: {
            hasSubfields: false,
            type: ['string'],
            mandatory: false
          }
        },
        phone: {
          meta: {
            hasSubfields: false,
            type: ['string'],
            mandatory: false
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
