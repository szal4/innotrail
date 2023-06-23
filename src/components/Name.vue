<template>
  <div class="container">

   <div class="flex-item">
     <p class="title">Great! Almost There </p>
   </div>

   <div class="flex-item option">
      <p class="option-name">What should we call you?</p>
      <input v-model="nameInput" class="option-input">
    </div>
    <div class="flex-item option">
      <p class="option-name">Please Provide your mobile number</p>
      <!-- <input class="option-input"> -->
      <div class="input-container">
        <input v-model="countryCodeInput" type="text" class="country-code" id="country-code" placeholder="+1" maxlength="4">
        <input v-model="mobileNumberInput" type="text" class="phone-number" placeholder="1234567890" maxlength="10">
      </div>

    </div>
    <div class="flex-item option">
      <button id="btn" @click="sendData"> Next </button>
    </div>

 </div>
</template>

<style scoped>
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
.option{
  text-align: left;
}
.option-name{
  margin-top: 20px;
  color:white;
  font-size: 14px;
  font-weight: 700;
}
.option-input{
  margin-top: 15px;
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
.input-container {
  display: flex;
  margin-top:15px;
}

.country-code {
  width: 60px; 
  height: 50px;
  padding: 10px;
  border:none;
  border-top-left-radius: 10px;
  border-bottom-left-radius: 10px;
}

.phone-number {
  flex-grow: 1;
  width: 200px;
  padding: 10px;
  border:none;
  border-top-right-radius: 10px;
  border-bottom-right-radius: 10px;
}
input{
  padding: 15px;
}
input:focus{
  outline: none;
}


</style>

<script scoped>
export default {
data() {
 return {
   tab:3,
   nameInput: "",
   countryCodeInput: "",
   mobileNumberInput: ""
 };
},
methods: {
  sendData() {
   this.sendDataToParent();
   var url = 'https://4caeisr4q3.execute-api.us-east-1.amazonaws.com/dev/mentoapp/login';
   const headers = {
        'Content-Type': 'application/json',
        'appflavour' : 'DEV'
      };
    const body = {
      "tempUserId": "331aba72-1add-4adf-8d6c-f17e1e4f4a52", 
      "deviceId": "387c2863-6ee3-4a56-8210-225f774edade",
      "mobileCountryCode": this.countryCodeInput,
      "mobileCountry": "Ind",
      "mobileNumber": this.mobileNumberInput,
      "name": this.nameInput
    }
   fetch(url,{
      method: 'POST',
      headers: headers,
      body: JSON.stringify(body)
    }).then((response) => {
          response.json().then((data) => {
            console.log(data);
       });
     })
     .catch((err) => {
       console.error(err);
     });
 },
 sendDataToParent() {
   this.$emit('custom-name-event', this.tab);
 }
},
created() {
 // Call function within the created lifecycle
//  this.fetchData();
 
//  document.addEventListener('DOMContentLoaded', () => {
//  const countryCodeInput = document.getElementById('country-code');
//  console.log(countryCodeInput);
//   const flagIcon = document.createElement('span');
//   flagIcon.classList.add('flag-icon');

// countryCodeInput.addEventListener('input', () => {
//   const countryCode = countryCodeInput.value;
//   const countryCodeWithoutPlus = countryCode.replace('+', '');
//   flagIcon.classList.remove(`flag-icon-${flagIcon.dataset.countryCode}`);
//   flagIcon.dataset.countryCode = countryCodeWithoutPlus;
//   flagIcon.classList.add(`flag-icon-${countryCodeWithoutPlus}`);
// });

// countryCodeInput.parentNode.appendChild(flagIcon);
//  })
}
}
</script>