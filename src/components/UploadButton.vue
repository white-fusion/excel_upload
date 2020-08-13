<template>
  <div class="button_group">
    <h1>{{ msg }}</h1>
    <input type="file" class="my_input" @change="importExcel" id="upload" accept=".xls, .xlsx"/>
  </div>
</template>

<script>
import xlsx from "xlsx";
export default {
  name: 'UploadButton',
  props: {
    msg: String,
    dataStruct: Object
  },
  methods: {
    getHeaders() {
    },
    importExcel(e) {
      const files = e.target.files;
      console.log(files);
       if (!files.length) {
        return ;
      }
      const fileReader = new FileReader();
      fileReader.onload = ev => {
        try {
          const data = ev.target.result;
          const XLSX = xlsx;
          const workbook = XLSX.read(data, {
            type: "binary"
          });
          const wsname = workbook.SheetNames[0]; //Taking name of the first sheet in the sheets
          const ws = XLSX.utils.sheet_to_json(workbook.Sheets[wsname]); // Generate JSON table content from the data of the first sheet
          // const excellist = [];  
          // // Edit data considering data structure
          // for (var i = 0; i < ws.length; i++) {
          //   for(dataStruct)
          // }
          console.log("Read results", ws); // an array containing objects that need to be processed
        } catch (e) {
          return alert("Read failure!");
        }
      };
      fileReader.readAsBinaryString(files[0]);
      var input = document.getElementById("upload");
      input.value = "";
    }
  }
}
</script>


<style scoped>

</style>
