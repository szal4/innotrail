<template>
     <div v-if="goalData" class="container">

      <div class="flex-item">
        <p class="title"> {{ goalData?.domainSelectionMessage }}!</p>
      </div>
      <div class="flex-item">
        <p class="dis"> {{ goalData?.domainSelectionMessageDetails }}!</p>
      </div>
      <div class="flex-item option-container">
        <div class="option" @click="sendDataToParent">
          <!-- v-bind:style= "[condition ? {styleA} : {styleB}]" -->
          <div id="option-one-icon" class="option-icon" v-bind:style="[IconURL1? {'background-image': 'url('+ IconURL1+')'} : {}]"></div>
          <p class="option-name">{{ goalData?.domains[0].label }}</p>
          <p class="option-dis">{{ goalData?.domains[0].description }}</p>
        </div>
        <div class="option" @click="sendDataToParent">
          <div id="option-two-icon" class="option-icon" v-bind:style="[IconURL2? {'background-image': 'url('+ IconURL2+')'} : {}]"></div>
          <p class="option-name">{{ goalData?.domains[1].label }}</p>
          <p class="option-dis">{{ goalData?.domains[1].description }}</p>
        </div>
      </div>

    </div>
    <div v-else class="loading" style="font-size: 20px; font-weight: 600;">
      Loading...
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
    font-weight: 600;
    color:white;
    font-size: 20px;
  }
  .dis{
    width: 300px;
    color:white;
  }
  .option-container{
    margin-top: 10px;
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
  }
  .option{
    background-color: white;
    border-radius: 10px;
    margin: 20px 20px;
    width: 240px;
    height: 150px;
    text-align: left;
    padding-left: 20px;
    padding-top: 20px;
    cursor: pointer;
  }
  .option:hover{
    background-color: rgb(232, 229, 238);
  }
  .option-icon{
    width: 45px;
    height: 45px;
    background-repeat: no-repeat;
    background-size: cover;
    border-radius: 50%;
    background-position: center;
  }
  .option-name{
    margin-top:10px;
    font-weight: 600;
  }
  .option-dis{
    font-size: 12px
  }
</style>

<script scoped>
import dayImg from "../image/day.png";
import nightImg from "../image/night.png";
export default {
 data() {
    return {
      dayImg : dayImg,
      nightImg : nightImg,
      goalData: "",
      tab:1,
      IconURL1: 'https://icon-library.com/images/sat-icon/sat-icon-12.jpg',
      IconURL2: 'https://icon-library.com/images/sat-icon/sat-icon-12.jpg'
    };
  },
   methods: {
    fetchData() {
      var url = 'https://4caeisr4q3.execute-api.us-east-1.amazonaws.com/dev/mentoapp/onboarding/start';
      const headers = {
        'Content-Type': 'application/json',
        'appflavour' : 'DEV',
        'appVersion'  : '2.5.0'
      };
      const body = {
        "newUser":true,
        "deviceId": "387c2863-6ee3-4a56-8210-225f774edade",
        "appFlavour":"DEV",
        "versionNumber":"2.5.0"
      };
      fetch(url,{
      method: 'POST',
      headers: headers,
      body: JSON.stringify(body)
    }).then((response) => {
          response.json().then((data) => {
            //console.log(data);
            this.goalData = data;
            this.IconURL1 = data.appurls.s3url+'/' + data.domains[0].image;
            this.IconURL2 = data.appurls.s3url+'/' + data.domains[1].image;
            //console.log(this.IconURL1)

            const hours = new Date().getHours()
            const isDayTime = hours > 6 && hours < 20;
            if(isDayTime === true){
              document.body.style.backgroundImage=`url(${dayImg})`;
            } else {
              document.body.style.backgroundImage=`url(${nightImg})`;
            }

           
            document.body.style.backgroundRepeat = 'no-repeat';
            document.body.style.backgroundAttachment = 'fixed';
            document.body.style.backgroundSize = 'cover';
            document.body.style.backgroundPosition = 'center'
            // console.log(this.goalData)
            // option one icon
            // let optioOneIconURL = 'https://icon-library.com/images/sat-icon/sat-icon-12.jpg'
            // document.getElementById('option-one-icon').style.backgroundImage=`url(${optioOneIconURL})`;
            
            // option two icon
            // let optioTwoIconURL = 'https://icon-library.com/images/sat-icon/sat-icon-12.jpg'
            // document.getElementById('option-two-icon').style.backgroundImage=`url(${optioTwoIconURL})`;
          });
        })
        .catch((err) => {
          console.error(err);
        });
    },
    sendDataToParent() {
      this.$emit('custom-goal-event', this.tab);
    }
  },
  created() {
    // Call function within the created lifecycle
    this.fetchData();
  }
}
</script>


<!-- var data =
{
  "appurls": {
      "version": "0",
      "apiurl": "https://api-app.devtest.thestudypod.com",
      "s3url": "https://mysa-img.devtest.thestudypod.com",
      "boturl": "https://aibot.devtest.thestudypod.com/mobile-dev.html",
      "organizationId": "1",
      "botConfigurationUrl": "https://innotrail-test-bot.s3.amazonaws.com/mentobot/mentoChatbotConfig-dev.json",
      "s3privateUrl": "https://vid.devtest.thestudypod.com"
  },
  "tempUserId": "628b4b4b-ffec-4550-b05d-3523f35c37b7",
  "status": "okToLogin",
  "domainSelectionMessage": "Choose Your Goal",
  "domainSelectionMessageDetails": "We will keep you motivated to acheive your goals",
  "domains": [
      {
          "key": "e_1685979314728",
          "label": "Digital SAT",
          "description": "Prepare to join top colleges in US",
          "image": "Entities/domain/1686440380123-ba764344c502513124c791b47dd00437.png"
      },
      {
          "key": "e_3",
          "label": "Engineering",
          "description": "Prepare to join top colleges in India",
          "image": "Entities/domain/1686239627422-cbse.png"
      }
  ]
} -->