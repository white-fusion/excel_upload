<template>
  <div class="button_group">
    <input type="file" class="my_input" @change="importExcel" id="upload" accept=".xls, .xlsx"/>
  </div>
</template>

<script>
import xlsx from "xlsx";
export default {
  name: 'UploadButton',
  props: {
    dataStruct: {
      type: Object
    }
  },
  data() {
    return {
      mand: [] //Don't forget to free the array after the work.
    }  
  },
  methods: {
    getMandFields(structure) {
      Object.keys(structure).forEach(dummy => {
        if(structure[dummy].meta.column && structure[dummy].meta.mandatory){ 
          this.mand.push(dummy);
        }else {
          if(structure[dummy].fields){
            this.getMandFields(structure[dummy].fields)
          } 
        }
      });
    },
    convertData(obj, structure){
      //assume all keys in obj have lower case by now
      //assume mandatory check has been done by now
      //Dont miss the case where that field is not mandatory and theres no data about it
      var converted = structure;
      for(const dummy in converted){
        if(converted[dummy].meta.column){
          if(Object.keys(obj).includes(dummy.toLowerCase())){
            //type check
            //var typeArray = converted[dummy].meta.type;
            //if (!typeArray.includes(typeof(obj["dummy.toLowerCase"])))
            //{return alert;}
            converted[dummy] = obj["dummy.toLowerCase"];
          } else{
            converted[dummy] = null;
          }
        } else{ //if it's not a column
          this.convertData(obj, converted[dummy]);
          delete converted[dummy].meta;
          delete converted[dummy].fields;
        }
      }
    },
    importExcel(e) {
      const files = e.target.files;
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
          //ws is an array of objects with each object representing each row of the excel sheet
          
          const headers = Object.keys(ws[0]); //might have to change this because if one header has no value for this row, that will be ignored
          const lowHeaders = headers.map(header => header.toLowerCase());
          console.log(lowHeaders);

          this.getMandFields(this.dataStruct); 
          
          const lowMand = this.mand.map(e => e.toLowerCase());
          console.log(lowMand);
          // lowMand.forEach(function(entry){
          //   if(!lowHeaders.includes(entry)){              
          //     return alert("Mandaatory field " + entry + " not present in the file!");//after alert it shouldn't continue functioning anymore
          //   } 
          // });

          const excellist = []; //To edit received data
          for (var j = 0; j < ws.length; j++) {
            excellist.push(ws[j]);
            //Checking type

            //Here convert it to the required datastructure
          }
          console.log("Read results", excellist); // an array containing objects that need to be processed
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
