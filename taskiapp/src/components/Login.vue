<template>
    <div class="login">
        <b-button variant="outline-primary">
            <router-link to="/">
                <img id="back" src="../assets/back.png">
            </router-link>
        </b-button>
        <h2>Welcome back</h2>
        <h5>Please log in :)</h5>
        <img src="../assets/login-pic.png" id="img" class="cup">
        <h6>{{msg}}</h6>
        <div id="email">
            <input type="email" placeholder="Email" v-model="email" >
        </div>
        <div id="psd">
            <input type="password" placeholder="Password" v-model="password">
        </div>
        <div>
            <b-button @click="login" class="btn log">
                LOGIN
            </b-button>
        </div>
        <div>
            <a>Forget Password?</a>
        </div>
        <div>
            <b-button variant="outline-primary" v-show="showSignUp" class="btn snup">
                <router-link to="/SignUp">
                    <p id="">CREATE ACCOUNT</p>
                </router-link>
            </b-button>
        </div>

        <!-- CHECK -->
        <div v-show="showID">
            <p>Hello, {{name}}</p>
            <p>UserId：{{nameId}}</p>
        </div>

    </div>
</template>
<script>
export default {
    name: "login",
    data(){
        return{
            msg: '',
            name: '',
            email: '',
            password: '',
            nameId: "",
            showID: false,
            array: [],
            showAll: false,
            showSignUp: true
        }
    },
    methods:{
        login(){
            this.showAll = false
            this.showID = false
            let params = {
                email: this.email,
                password: this.password
            }
            const regEmail = /^([a-zA-Z0-9]+[-_\.]?)+@[a-zA-Z0-9]+\.[a-z]+$/;
            if (this.email == '' || this.email == undefined || this.email == null
                || this.password == '' || this.password == undefined || this.password == null ) {
                this.msg = "The input box cannot be empty ";
                return;
            } else {
                if (!regEmail.test(this.email)) {
                    this.msg = "Please input the correct format in the input box ";
                    return;
                }
            }

            this.$http.post('/api/user/login',params).then((res)=>{
                console.log(res)
                if(res.data.status == 1000){
                   this.showID = true
                   this.nameId = res.data.data[0]._id
                   this.name = res.data.data[0].name
                   this.showSignUp = false;
                   this.GLOBAL.userId = res.data.data[0]._id;
                   this.GLOBAL.userName = res.data.data[0].name;
                   this.GLOBAL.userEmail = res.data.data[0].email;
                   this.$router.push({
                       path: '/Home',
                       replace: true
                    });
                }else{
                    //alert(res.data.message)
                    this.msg = res.data.message
                }
            }).catch((err)=>{
                console.log(err)
            })
        }
    }
    ,
    created: function() {
          if(this.GLOBAL.userId != '' ){
              this.$router.push({
                path: '/Home',
                replace: true
              });
          }
    }
}
</script>


<style scoped>
h2{
    font-size:36px;
    font-weight:800;

}
h5{
    font-size:24px;
}
h6{
    font-size:16px;
    color: red;
}
h2,h5{
    text-align:left;
    margin-left:14%;
    color:#1B052F;
}
p{
    color:#2E0E4C;
}
#back{
    position:absolute;
    left:12%;
    top:6%;
}

.btn{
    width: 280px;
    height: 40px;
    border:none;
}
.log{
    background:linear-gradient(to right, #04B4EE,#1ED9D3);
    color:white;
    margin-top:8%;
    margin-bottom:6%;

}
.snup{
    border:2px solid transparent;
    border-image:linear-gradient(to right, #04B4EE,#1ED9D3);
    border-image-slice:1;
    background-color:white;
    border-radius:8px;
    margin-top:6%;
}
.cup{
    width: 100px;
    margin-top:4%;
}
#email{
    margin-top: 5%;
    margin-bottom: 6%;
}
#psd{
    margin-top: 2%;
    margin-bottom: 6%
}
#email input{
    border: none;
    outline: none;
    border-bottom: 1px solid #ccc;
    width:280px;
}
#psd input{
    border: none;
    outline: none;
    border-bottom: 1px solid #ccc;
    width:280px;
}

</style>
