<template>
        <v-container class="signupform">
            <br><br><br>
            <h1 class="text-center" >User Sign Up</h1>
            <br/>
            <v-form v-model="isFormValid">
            <v-alert dismissible type="error" v-model="fail"> Duplicate User Email </v-alert>
            <v-col>
                <v-row>
                    <v-col>
                        <v-text-field required v-model="user.name" label="Full Name" :rules="[rules.required, rules.name]"></v-text-field>
                    </v-col>
                </v-row>
                <v-row>
                    <v-col>
                        <v-text-field v-model="user.email" label="Personal Email" :rules="[rules.required,rules.email]"></v-text-field>
                    </v-col>
                </v-row>
                <v-row>
                    <v-col>
                        <v-text-field v-model="user.password" label="Password" type="password" :rules="[rules.required,rules.min]"></v-text-field>
                    </v-col>
                </v-row>
            </v-col>
            <v-container class="text-center">
            <v-btn  color="indigo darken-3" elevation="4" @click="submit()" :disabled="!isFormValid" class="button"> Submit</v-btn>
            </v-container>
            </v-form>
            <br/><br/>
            <v-container v-if="sendotp">
                <v-form>
                    <v-alert v-model="error" type="error"  dismissible>OTP verification failed</v-alert>
                    <v-col>
                        <v-row>
                            <caption> An OTP is send to your provided email. Please enter the OTP below for verification</caption>
                        </v-row>
                        <v-row>
                            <v-text-field label ="Enter OTP Here" v-model="utop"></v-text-field>
                        </v-row>
                        <v-row>
                            <v-btn large block color="indigo darken-3" @click="signup()" class="button">signup</v-btn>
                        </v-row>
                    </v-col>
                </v-form>
            </v-container>
        </v-container>
    
</template>
<script>

export default{
    name:'signuppage',
    layout:'signinlayout',
    async mounted(){
        this.$vuetify.theme.dark=false;
    },
    data:() => ({
        user :{
            name :'',
            email :'',
            password :'',
            firstlogin :true,
            status: 'pending'
        },
        success:false,
        fail:false,
        otp:'',
        utop:'',
        isFormValid:null,
        error:false,
        sendotp:null,
        rules:{
            required: (v) => !!v || "Required",
            min : (v) =>  v.match(/^(?=.*[A-Z])(?=.*[@])(?=.*[a-z])(?=.*\d).{8,}$/)|| "Enter Password with One Cap letter ,@,small letter and with the number is required",
            email : (v) => v.match(/\S+@\S+\.\S+/) || "Email format is wrong",
            name: (v) => v.match(/^[A-Za-z\s]+$/) || "No special Characters in Name"
        }
    }),
    methods:{
        async home(){
            this.$router.push('/')
        },
        async submit(){
            let url = "http://3.84.79.77:8000/otp"
            let mdata = { params :{email : this.user.email}}
            await this.$axios.get(url,mdata).then(res => {
                this.otp = res.data
                this.sendotp = true
                console.log(this.otp)
            }).catch(err => { console.log(err)});
        },
        async signup(){
            if (this.utop == this.otp){
                let url = "http://127.0.0.1:8000/user"
                await this.$axios.post(url,this.user).then(res => {
                    if (res.data == true){
                        this.success = true
                        this.$router.push('/signin')
                    }
                    else{
                        this.fail = true
                    }
                });
            }
            else {
                this.error = true
            }
        }
    }
}
</script>
<style>
.signupform{
    width: 30%;
    margin: 0% auto;
}

</style>
