<template>
    <v-main class="grey lighten-3">
      <v-container>
        <v-row>
          <v-col cols="3">

              <v-container fluid>
                <v-card max-width="450px" class="mx-auto bg" elevation="2">
                  <br><br>
                  <v-row justify="center">
                    <v-col align-self="start" class="d-flex justify-center align-center pa-0" cols="12">
                      <v-avatar class="profile avatar-center-heigth avatar-shadow" color="grey" size="164">

                        <input ref="uploader" class="d-none" type="file" accept="image/*" :change="onFileChanged">
                        <v-img src="https://cdn.vuetifyjs.com/images/profiles/marcus.jpg"></v-img>


                      </v-avatar>

                    </v-col>
                    <v-btn @click="onButtonClick" class="upload-btn" x-large icon>
                      <v-icon>
                        mdi-camera
                      </v-icon>
                    </v-btn>
                  </v-row>
                  <v-row>
                    <userbanner/>
                    <personprofile/>
                  </v-row>

                </v-card>
              </v-container>
              <v-container>
                <v-card max-width="450px" class="mx-auto bg" elevation="2">
                  <AddressPage/>
                </v-card>
              </v-container>

          </v-col>

          <v-col>
      <personaldetails/>

      <sslcview/>

      <hseview/>

      <ugview/>

      <pgview/>

      <expview/>

          </v-col>
          <v-col cols="3">

            <v-container fluid>
              <v-card max-width="350px" height="420px" class="mx-auto bg" elevation="2">
                <br>
                <v-row justify="center">
                  <v-card-title>Profile</v-card-title>
                </v-row>
                <v-container>
                      <v-container>
                        <v-list>
                          <v-list-item>
                            <v-list-item-title>Current visiting time</v-list-item-title>
                          </v-list-item>
                          <v-list-item>
                            <v-list-item-subtitle>
                              {{ pdata.notary_last_visited }}
                            </v-list-item-subtitle>
                          </v-list-item>
                          <v-list-item>
                            <v-list-item-title>Last visiting time</v-list-item-title>
                          </v-list-item>
                          <v-list-item>
                            <v-list-item-subtitle>
                              {{ pdata.notary_last_visited }}
                            </v-list-item-subtitle>
                          </v-list-item>
                        </v-list>

                      </v-container>
                </v-container>

              </v-card>
            </v-container>

        </v-col>
        </v-row>
      </v-container>
    </v-main>
</template>
<script>
export default{
    name: 'profile',
    layout:'notary_layout',
    async mounted (){
        this.$vuetify.theme.dark =false;
        this.email = this.$storage.getUniversal('user_email')
        let url = "http://127.0.0.1:8000/user"
        let res = await this.$axios.get(url,{params:{ email :this.email}});
        this.name=res.data.name

        this.email = this.$storage.getUniversal('user_email')
        let nurl = "http://127.0.0.1:8000/user"
        let nres = await this.$axios.get(nurl,{params:{ email :this.email}});
        this.pdata = nres.data
        console.log(this.pdata)
    },
    data: () =>({
        name : "Sajith Surendran",
        email:"",
        pdata:{}
    }),
   methods: {

   }
}
</script>
