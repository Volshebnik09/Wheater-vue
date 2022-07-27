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
      <currentStats
          :pressure="pressureMb"
          :humidity="humidity"
          :windDirection="windDirection"
          :windPower="windPowerMph"
      />
    </div>

    <graph
        :temperature="forecastZeroTemperature"
        :chanceOfRain="forecastZeroRainChance"
        class="current__graph"
    />


  </div>
  <div class="forecast">
    <div class="forecast-day" v-for="day in forecast">
      <div class="container">
        <div class="top">
          <div class="date">{{day.date}}</div>
        </div>
          <div class="mid">
            <div class="hourStats" v-for="n in [0, 6, 12, 23]">
              <img :src="day?.hour[n]?.condition.icon || ''" alt="">
              <div class="hourTemp">
                {{checkTemp(day?.hour[n]?.temp_c)}}
              </div>
              <div class="hourRain">
                {{day?.hour[n]?.chance_of_rain}}%
              </div>
            </div>
          </div>
      </div>
    </div>
  </div>
</template>

<script>
import top from "@/components/top"
import currentMid from "@/components/current-mid"
import currentStats from "@/components/current-stats";
import graph from "@/components/Graph";
import axios from "axios";

export default {
  components:{
    top,currentMid,currentStats,graph
  },
  created(){
    let params = new URLSearchParams(window.location.search);
    if (params.has('search')){
      this.location= params.get('search')
    }

    this.sendRequest(this.location)
  },
  methods:{
    checkTemp(temp){
      if (temp>0)
      return "+ " + temp
    },
    sendRequest(val){
      axios
          .get('http://api.weatherapi.com/v1/forecast.json?key=af2cf1a57a5b486b8b7173931221007&q='+val+'&days=4&aqi=no&alerts=no')
          .then(response=>{
            console.log(response.data)
            this.currentTemp = response.data.current.temp_c > 0 ? "+" + response.data.current.temp_c : response.data.current.temp_c
            this.currentSky = response.data.current.condition.icon
            this.pressureMb = response.data.current.pressure_mb
            this.humidity = response.data.current.humidity
            this.windDirection = response.data.current.wind_dir
            this.windPowerMph = response.data.current.wind_mph
            this.forecastZeroTemperature = []
            this.forecastZeroRainChance=[]
            this.forecast = response.data.forecast.forecastday.slice(1, 7)
            console.log(this.forecast)
            response.data.forecast.forecastday[0].hour.forEach((el)=>{
              this.forecastZeroTemperature.push(el.temp_c)
            })
            response.data.forecast.forecastday[0].hour.forEach((el)=>{
              this.forecastZeroRainChance.push(el.chance_of_rain)
            })

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
      pressureMb : '?',
      humidity: '?',
      windPowerMph: '?',
      windDirection: '?',
      forecastZeroTemperature: [],
      forecastZeroRainChance:[],
      forecast: [],
    }

  },

}

</script>
<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800&display=swap');
#app {
  max-width: 400px;
  margin: auto;
  background: white;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.current {
  background: linear-gradient(180deg, #000000 -32.39%, #153470 50.04%, #000000 138.91%);
  border-bottom-left-radius: 45px;
  border-bottom-right-radius: 45px;
  position: relative;
  padding-bottom: 190px;
}
.current__graph {
  width: 90%;
  background: #c8c8c8;
  border-radius: 25px;
  box-shadow: #00000070 -5px 5px 15px;
  position: absolute;
  bottom: -30%;
  left: 50%;
  transform: translateX(-50%);
  padding-top: 10px;
}

.forecast {
  font-family: Montserrat;
  .forecast-day{
    margin: 10px 0;
    background: #f5f5f5;
    padding-top: 15px;
    border-bottom: 1px solid black;


  }
  .forecast-day + .forecast-day{
    border-top: 1px solid black;
    border-bottom: 0px solid black;

  }
  padding-top: 120px;
  .container{
    width: 90%;
    margin: auto;
  }
  .mid {
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .hourStats{
    padding: 5px 10px 10px;
    display: flex;
    align-items: center;
    flex-direction: column;
  }
  .hourTemp{
    padding: 3px 0;
  }

}


</style>