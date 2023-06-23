<template>
  <div class="mainApp">
    <h1 id="top-title">MentoMind</h1>
    <div class="tabs">
      <Goal v-if="tab==0" @custom-goal-event="handleCustomEvent"/>
      <Skill v-if="tab==1"  @custom-skill-event="handleCustomEvent"/>
      <Name v-if="tab==2"  @custom-name-event="handleCustomEvent"/>
      <Otp v-if="tab==3"  @custom-otp-event="handleCustomEvent"/>
    </div>

  </div>
</template>
<script>
import Goal from './components/Goal.vue';
import Skill from './components/Skill.vue';
import Name from './components/Name.vue';
import Otp from './components/Otp.vue'
export default {
  components: {
    Goal,
    Skill,
    Name,
    Otp
  },
  props: {
    msg: String,
  },
  data() {
    return {
      tab: 0,
    };
  },
  methods: {
    handleCustomEvent(data) {
      this.tab = data;
    }
  },
};
</script>
<style>

#top-title{
  font-size: 20px;
  color : white;
  padding: 18px;
  height: 10vh;
  font-weight: 600;
}
.tabs{
  display: flex;
  justify-content: center;
  align-items: center;
  height: 65vh;
}

</style>