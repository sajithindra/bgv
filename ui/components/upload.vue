<template>
  
    <v-container fluid class="d-flex align-center justify-center">
      <v-card class="text-center" :elevation="6" :height="auto" :width="700">
              <br>
              <v-text-title><h2>User Data</h2></v-text-title>
              <br>
              <v-row>
                  <v-col>
                      <v-container >
                        <v-container style="width: 100%; ">
                          <v-btn size="40%" color="indigo darken-4" style="color: white" @click="downloadCSVTemplate">Download Template
                         </v-btn> 
                       </v-container>
                      </v-container>
                  </v-col>
              </v-row>
              <v-divider ></v-divider>
              <v-divider ></v-divider>

              <v-divider></v-divider>
              <v-row>
                <v-col>
                  <v-container class="text-center">
                    <v-file-input @change="fileselect" style="width:70%; margin:0 auto; " label="File input" variant="solo-filled"></v-file-input>
                    <v-btn size="20%"  :loading="isLoading" :disabled="isLoading" color="indigo darken-4" style="color:white" @click="upload()">Upload</v-btn>
                  </v-container>
                  
                </v-col>
              </v-row>
              <v-row>     
                <v-container class="text-center">
                  <h3 v-if="this.data.total_count">Uploaded Details:</h3>
                </v-container>               
              </v-row>

                          </v-card>
        </v-container>
</template>
<script>
export default {
  name: 'hseupload',
  data: () => ({
      data:{},
      isLoading: false

    }),
  methods:{
    async fileselect(event){
        this.file=event
      },
    async upload(){
        let formdata= new FormData()
            formdata.append('csv_file',this.file)
            let furl = "http://127.0.0.1:8000/hr/upload"
            let res = await this.$axios.post(furl, formdata);
            this.data = res.data
            console.log(res.data)

            this.isLoading = true;
            // Simulate an asynchronous operation, such as an API call
            setTimeout(() => {
              // After the operation is complete, set isLoading to false
              this.isLoading = false;
              location.reload();
            }, 2000);
    
  },
   
    downloadCSVTemplate() {
      const csvContent ="name,email,hse_regno,hse_marks,hse_passout,hse_school,hse_board"
      const blob = new Blob([csvContent], { type: "text/csv;charset=utf-8;" });
      const url = URL.createObjectURL(blob);
      const link = document.createElement("a");
      link.href = url;
      link.download = "Template.csv";
      link.style.visibility = "hidden";
      document.body.appendChild(link);
      link.click();
      document.body.removeChild(link);
      URL.revokeObjectURL(url);
    },
  
  }
  
};
</script>
