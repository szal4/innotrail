<template>
  <div v-if="skillData" class="container">

   <div class="flex-item">
     <p class="title">{{ skillData[0].questionText }}</p>
   </div>

   <div class="flex-item option-container">

    <div class="coloumn">
      <div class="option" @click="sendDataToParent">
        <div id="option-one-icon" class="option-icon" v-bind:style="[IconURL1? {'background-image': 'url('+ IconURL1+')'} : {}]"></div>
        <p class="option-name"> {{ skillData[0].options[0].text }} </p>
      </div>
      <div class="option" @click="sendDataToParent">
        <div id="option-two-icon" class="option-icon" v-bind:style="[IconURL2? {'background-image': 'url('+ IconURL3+')'} : {}]"></div>
        <p class="option-name">{{ skillData[0].options[2].text }} </p>
      </div>
    </div>

    <div class="coloumn">
      <div class="option" @click="sendDataToParent">
        <div id="option-three-icon" class="option-icon" v-bind:style="[IconURL3? {'background-image': 'url('+ IconURL2+')'} : {}]"></div>
        <p class="option-name">{{ skillData[0].options[1].text }}</p>
      </div>
      <div class="option" @click="sendDataToParent">
        <div id="option-four-icon" class="option-icon" v-bind:style="[IconURL4? {'background-image': 'url('+ IconURL4+')'} : {}]"></div>
        <p class="option-name">{{ skillData[0].options[3].text }}</p>
      </div>
    </div>

   </div>

 </div>
 <div v-else class="loading" style="font-size: 20px; font-weight: 600;color: white;">
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
@media screen and (max-width: 600px) {
  .container {
    margin-top: 120px;
  }
}
.title{
 text-align: center;
 font-size: 20px;
 margin-top: 30px;
 font-weight: 600;
 color:white;
 width: 340px;
}
.option-container{
 display: flex;
 flex-wrap: wrap;
 justify-content: space-around;
}
.option{
   background-color: white;
   border-radius: 10px;
   margin: 20px 20px;
   width: 170px;
   height: 110px;
   text-align: center;
   padding: 12px;
   cursor: pointer;
}
.option:hover{
    background-color: rgb(232, 229, 238);
  }
.option-icon{
 width: 40px;
 height: 40px;
 margin-left: 53px;
 background-repeat: no-repeat;
 background-size: cover;
}
.option-name{
  font-size: 12px;
  margin-top:8px;
  font-weight: 800;
}

</style>

<script scoped>
export default {
data() {
 return {
   skillData: "",
   tab: 2,
   IconURL1: 'https://icon-library.com/images/sat-icon/sat-icon-12.jpg',
   IconURL2: 'https://icon-library.com/images/sat-icon/sat-icon-12.jpg',
   IconURL3: 'https://icon-library.com/images/sat-icon/sat-icon-12.jpg',
   IconURL4: 'https://icon-library.com/images/sat-icon/sat-icon-12.jpg'
 };
},
methods: {
 fetchData() {
   var url = 'https://4caeisr4q3.execute-api.us-east-1.amazonaws.com/dev/mentoapp/onboarding/domain';
   const headers = {
        'Content-Type': 'application/json',
        'appflavour' : 'DEV'
      };
      const body = {
        "tempUserId": "331aba72-1add-4adf-8d6c-f17e1e4f4a52",
        "selectedDomainId":"e_1685979314728"
      };
   fetch(url,{
      method: 'POST',
      headers: headers,
      body: JSON.stringify(body)
    }).then((response) => {
          response.json().then((data) => {
            //console.log(data);
            this.skillData = data;
            this.IconURL1 = 'https://mysa-img.devtest.thestudypod.com/'+ data[0].options[0].image
            this.IconURL2 = 'https://mysa-img.devtest.thestudypod.com/'+ data[0].options[1].image
            this.IconURL3 = 'https://mysa-img.devtest.thestudypod.com/'+ data[0].options[2].image
            this.IconURL4 = 'https://mysa-img.devtest.thestudypod.com/'+ data[0].options[3].image
            //console.log(this.skillData);
        //  // option one icon
        //  let optioOneIconURL = 'https://icon-library.com/images/sat-icon/sat-icon-12.jpg'
        //  let optioOneIconEle = document.getElementById('option-one-icon');
        //  optioOneIconEle.style.backgroundImage=`url(${optioOneIconURL})`;
         
        //  // option two icon
        //  let optioTwoIconURL = 'https://icon-library.com/images/sat-icon/sat-icon-12.jpg'
        //  let optioTwoIconEle = document.getElementById('option-two-icon');
        //  optioTwoIconEle.style.backgroundImage=`url(${optioTwoIconURL})`;

        //  // option three icon
        //  let optionThreeIconURL = 'https://icon-library.com/images/sat-icon/sat-icon-12.jpg'
        //  let optionThreeIconEle = document.getElementById('option-three-icon');
        //  optionThreeIconEle.style.backgroundImage=`url(${optionThreeIconURL})`;

        //  // option four icon
        //  let optioFourIconURL = 'https://icon-library.com/images/sat-icon/sat-icon-12.jpg'
        //  let optioFourIconEle = document.getElementById('option-four-icon')
        //  optioFourIconEle.style.backgroundImage=`url(${optioFourIconURL})`;
       });
     })
     .catch((err) => {
       console.error(err);
     });
 },
  sendDataToParent() {
    this.$emit('custom-skill-event', this.tab);
  }
},
created() {
 // Call function within the created lifecycle
 this.fetchData();
}
}
</script>

<!-- [
    {
        "domainId": "e_1685979314728",
        "questionId": "4",
        "questionText": "What's more important to improve your skills?",
        "options": [
            {
                "optionId": "1",
                "text": "Daily Practice",
                "image": "onboardingimages/daily_practice.svg"
            },
            {
                "optionId": "2",
                "text": "Track your skill progress",
                "image": "onboardingimages/track_your_skill_progress.svg"
            },
            {
                "optionId": "3",
                "text": "Track your speed",
                "image": "onboardingimages/track_your_speed.svg"
            },
            {
                "optionId": "4",
                "text": "All of the above",
                "image": "onboardingimages/all_of_the_above.svg"
            }
        ]
    },
    {
        "domainId": "e_1685979314728",
        "questionId": "5",
        "questionText": "How can you get better everyday?",
        "options": [
            {
                "optionId": "1",
                "text": "Challenge myself",
                "image": "onboardingimages/challenge_myself.svg"
            },
            {
                "optionId": "2",
                "text": "challenge my friends",
                "image": "onboardingimages/challenge_my_friends.svg"
            },
            {
                "optionId": "3",
                "text": "challenge my parents",
                "image": "onboardingimages/challenge_my_parents.svg"
            },
            {
                "optionId": "4",
                "text": "All of the above",
                "image": "onboardingimages/all_of_the_above.svg"
            }
        ]
    },
    {
        "domainId": "e_1685979314728",
        "questionId": "6",
        "questionText": "What are the benefits of using this app?",
        "options": [
            {
                "optionId": "1",
                "text": "Significant jump in sat score",
                "image": "onboardingimages/significant_jump_in_sat_score.svg"
            },
            {
                "optionId": "2",
                "text": "Win gift cards",
                "image": "onboardingimages/win_gift_cards.svg"
            },
            {
                "optionId": "3",
                "text": "Learn and have fun",
                "image": "onboardingimages/learn_and_have_fun.svg"
            },
            {
                "optionId": "4",
                "text": "All of the above",
                "image": "onboardingimages/all_of_the_above.svg"
            }
        ]
    }
] -->