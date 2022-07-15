<template>
  <div class="current">
    <top
        :searchQuery="location"
    />
    <div class="current__center">
      <currentMid
          :currentTemp="currentTemp"
          :currentSky="currentSky"
      />

    </div>



  </div>
</template>

<script>
import top from "@/components/top"
import currentMid from "@/components/current-mid"
import axios from "axios";

export default {
  components:{
    top,currentMid
  },
  created(){
    let params = new URLSearchParams(window.location.search);
    if (params.has('search')){
      this.location= params.get('search')
    }

    this.sendRequest(this.location)
  },
  methods:{
    sendRequest(val){
      axios
          .get('http://api.weatherapi.com/v1/forecast.json?key=af2cf1a57a5b486b8b7173931221007&q='+val+'&days=4&aqi=no&alerts=no')
          .then(response=>{
            console.log(response.data)
            this.currentTemp = response.data.current.temp_c > 0 ? "+" + response.data.current.temp_c : response.data.current.temp_c
            this.currentSky = response.data.current.condition.icon
          })
          .catch(error => {
            console.log(error)
            alert(error.response.data.error.message)
          })
    }
  },
  data(){
    return {
      location: "Tambov",
      currentTemp: "0_0",
      currentSky: "https://cdn.weatherapi.com/weather/64x64/day/113.png",
    }

  },

}

</script>
<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800&display=swap');
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.current {
  background: linear-gradient(180deg, #000000 -32.39%, #153470 50.04%, #000000 138.91%);
}



</style>