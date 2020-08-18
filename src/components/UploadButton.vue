<template>
  <div class="button_group">
    <input type="file" class="my_input" @change="importExcel" id="upload" accept=".xls, .xlsx"/>
  </div>
</template>

<script>
import xlsx from "xlsx";
import cloneDeep from "lodash.clonedeep"; 
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
    getHeader(sheet) {
      const XLSX = xlsx;
      const headers = [];
      const range = XLSX.utils.decode_range(sheet["!ref"]); // worksheet['!ref'] Is the valid range of the worksheet
      let C;
      /* Get cell value start in the first row */
      const R = range.s.r; //Line / / column C
      let i = 0;
      for (C = range.s.c; C <= range.e.c; ++C) {
        var cell =
          sheet[
            XLSX.utils.encode_cell({ c: C, r: R })
          ]; /* Get the cell value based on the address  find the cell in the first row */
        var hdr = "UNKNOWN" + C; // replace with your desired default
        // XLSX.utils.format_cell Generate cell text value
        if (cell && cell.t) hdr = XLSX.utils.format_cell(cell);
        if(hdr.indexOf('UNKNOWN') > -1){
          if(!i) {
            hdr = '__EMPTY';
          }else {
            hdr = '__EMPTY_' + i;
          }
          i++;
        }
        headers.push(hdr);
      }
      return headers;
    },
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
      Object.keys(structure).forEach(dummy => {
        if(structure[dummy].meta.column){
          if(Object.keys(obj).includes(dummy.toLowerCase())){
            // var typeArray = structure[dummy].meta.type;
            // if (!typeArray.includes(typeof(obj[dummy.toLowerCase()]))){
            //   return alert;
            // }
            structure[dummy] = obj[dummy.toLowerCase()];
          } else{ //If it is not mandatory and data is not there
            delete structure[dummy];
          }
        } else if(!structure[dummy].meta.column){//If it is not a column
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
          const a = workbook.Sheets[wsname];
          const ws = XLSX.utils.sheet_to_json(a); // Generate JSON table content from the data of the first sheet
          //ws is an array of objects with each object representing each row of the excel sheet
          
          
          
          const headers = this.getHeader(a);
          console.log('headers', headers);
          const lowHeaders = headers.map(header => header.toLowerCase().replace(/[ ,.-]/g, ""));//Converts names of headers to lower case and removes any spaces, dots, hyphens, commas
          console.log(lowHeaders);


          this.getMandFields(this.dataStruct);  
          const lowMand = this.mand.map(e => e.toLowerCase());
          console.log(lowMand);
          lowMand.forEach(function(entry){
            if(!lowHeaders.includes(entry)){              
              return alert("Mandatory field '" + entry + "' not present in the file!");//after alert it shouldn't continue functioning anymore
            } 
          });

          
          
          
          
         




          const excellist = []; //To edit received data
          for (var j = 0; j < ws.length; j++) {
            var key, keys = Object.keys(ws[j]);
            var n = keys.length;
            var obj={}
            while (n--) {
              key = keys[n];
              obj[key.toLowerCase().replace(/[ ,.-]/g, "")] = ws[j][key];
            }
            var structure = cloneDeep(this.dataStruct);
            this.convertData(obj, structure);
            excellist.push(structure);
          }
          //console.log("Read results", excellist); // an array containing objects that need to be processed
          var myJSON = JSON.stringify(excellist);
          console.log(myJSON);
        } catch (e) {
          //return alert("Read failure!");
          console.log(e);
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
