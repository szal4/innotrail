<template>
    <div class="container">
  
     <div class="flex-item">
       <p class="title">Great! Almost There</p>
     </div>

     <div class="flex-item" style="margin-top: 40px;">
       <p class="title">Enter OTP!</p>
     </div>

     <div class="flex-item">
        <p class="dis">OTP sent to provided number</p>
      </div>
  
     <!-- <div class="flex-item option">
        <input class="option-input">
      </div> -->
      <div class="mb-6 text-center">
  <div id="otp" class="flex justify-center">
    <input class="m-2 text-center form-control form-control-solid rounded focus:border-blue-400 focus:shadow-outline" type="text" id="first" maxlength="1" />
    <input class="m-2 text-center form-control form-control-solid rounded focus:border-blue-400 focus:shadow-outline" type="text" id="second" maxlength="1" />
    <input class="m-2 text-center form-control form-control-solid rounded focus:border-blue-400 focus:shadow-outline" type="text" id="third" maxlength="1" />
    <input class="m-2 text-center form-control form-control-solid rounded focus:border-blue-400 focus:shadow-outline" type="text" id="fourth" maxlength="1" />
    <input class="m-2 text-center form-control form-control-solid rounded focus:border-blue-400 focus:shadow-outline" type="text" id="fifth" maxlength="1" />
  </div>
</div>

      <div class="flex-item">
        <p class="dis">Did not receive yet? <span id="resend"> Resend </span></p>
      </div>
  
      
      <div class="flex-item option">
        <button id="btn" @click="sendData"> Next </button>
      </div>
  
   </div>
  </template>
  
  <style scoped>
  @import url('https://unpkg.com/tailwindcss@^1.0/dist/tailwind.min.css');
  .container{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  /* background-color: azure; */
  }
  .flex-item{
   /* background-color: rgb(151, 227, 192); */
   text-align: center;
   margin: 10px auto;
  }
  .title{
   text-align: center;
   font-size: 20px;
   font-weight: 600;
   color:white;
  }
  #resend{
    font-weight: 700;
    cursor: pointer;
  }
  .dis{
    width: 300px;
    color:white;
    font-size: 13px;
  }
  .option{
    text-align: left;
  }

  .option-input{
    margin-top: 8px;
    border-radius: 10px;
    height: 50px;
    width:260px;
    border:none;
  }
  #btn{
    color:white;
    font-size: 12px;
    border: none;
    background-color: #206ae0;
    height: 40px;
    width: 110px;
    border-radius: 20px;
    margin-top: 20px;
    cursor: pointer;
  }
  /* input{
  padding: 15px;
}
  input:focus{
    outline: none;
  } */
  .form-control {
  -webkit-transition: none;
  transition: none;
  width: 32px;
  height: 32px;
  text-align: center
}

.form-control:focus {
  color: #3F4254;
  background-color: #ffffff;
  border-color: #884377;
  outline: 0;
}

.form-control.form-control-solid {
  background-color: #F3F6F9;
  border-color: #F3F6F9;
  color: #3F4254;
  transition: color 0.15s ease, background-color 0.15s ease, border-color 0.15s ease, box-shadow 0.15s ease;
}

.form-control.form-control-solid:active,
.form-control.form-control-solid.active,
.form-control.form-control-solid:focus,
.form-control.form-control-solid.focus {
  background-color: #EBEDF3;
  border-color: #EBEDF3;
  color: #3F4254;
  transition: color 0.15s ease, background-color 0.15s ease, border-color 0.15s ease, box-shadow 0.15s ease;
}
  
  </style>
  
  <script scoped>
  export default {
  data() {
   return {
     goalData: "",
     tab:0
   };
  },
  methods: {
    
    sendData() {
     this.sendDataToParent();
     var url = 'https://api.publicapis.org/entries';
     fetch(url)
       .then((response) => {
         response.json().then((data) => {
           console.log(data);
           this.goalData = data;
           
  
         });
       })
       .catch((err) => {
         console.error(err);
       });
   },
   sendDataToParent() {
     this.$emit('custom-otp-event', this.tab);
   },
OTPInput() {
  const inputs = document.querySelectorAll('#otp > *[id]');
  for (let i = 0; i < inputs.length; i++) {
    inputs[i].addEventListener('keydown', function(event) {
      if (event.key === "Backspace") {
        inputs[i].value = '';
        if (i !== 0)
          inputs[i - 1].focus();
      } else {
        if (i === inputs.length - 1 && inputs[i].value !== '') {
          return true;
        } else if (event.keyCode > 47 && event.keyCode < 58) {
          inputs[i].value = event.key;
          if (i !== inputs.length - 1)
            inputs[i + 1].focus();
          event.preventDefault();
        } else if (event.keyCode > 64 && event.keyCode < 91) {
          inputs[i].value = String.fromCharCode(event.keyCode);
          if (i !== inputs.length - 1)
            inputs[i + 1].focus();
          event.preventDefault();
        }
      }
    });
  }
}
  },
  mounted() {
    this.OTPInput(); 
    //
  }
  }
  </script>