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
      //Mandatory checks should be done before calling this function
      console.log(obj);
      Object.keys(structure).forEach(dummy => {
        console.log(dummy);
        if(structure[dummy].meta.column){
          if(Object.keys(obj).includes(dummy.toLowerCase())){
            // var typeArray = structure[dummy].meta.type;
            // if (!typeArray.includes(typeof(obj[dummy.toLowerCase()]))){
            //   return alert;
            // }
            structure[dummy] = obj[dummy.toLowerCase()];
            console.log('Is a column');
            console.log(structure[dummy]);
          } else{ //If it is not mandatory and data is not there
            //structure[dummy] = null;
          }
        } else if(!structure[dummy].meta.column){//If it is not a column
          console.log('aint a column');
          structure[dummy] = structure[dummy].fields; //deletes meta and fields and directly bring up all subfields as an object
          this.convertData(obj, structure[dummy]);
        }
      });
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
          //     return alert("Mandatory field " + entry + " not present in the file!");//after alert it shouldn't continue functioning anymore
          //   } 
          // });

          
          
          
          
          
          var structure = this.dataStruct;
          var key, keys = Object.keys(ws[1]);
          var n = keys.length;
          var obj={}
          while (n--) {
            key = keys[n];
            obj[key.toLowerCase()] = ws[1][key];
          }
          this.convertData(obj, structure);
          console.log(structure);









          const excellist = []; //To edit received data
          for (var j = 0; j < ws.length; j++) {
            //var structure = this.dataStruct;
            //Do I need to return an object or is it dynamic
            //Checking type
            //this.convertData(ws[j], structure);

            //Here convert it to the required datastructure
            excellist.push(ws[j]);
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
