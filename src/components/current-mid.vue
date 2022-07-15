<template>
  <div class="mid">
    <div class="current-temp">
      {{currentTemp}}
    </div>
    <img :src="currentSky" alt="ы" class="current-sky">
  </div>
</template>

<script>
export default {
  name: "current-mid",
  props:[
      'currentTemp',
      'currentSky',
  ],
}
</script>

<style scoped lang="scss">
.mid {
  display: flex;
  align-items: center;
  justify-content: center;
  gap:25px;
  padding: 0 0 15px 0;
  .current-temp{

    font-family: 'Montserrat';
    font-style: normal;
    font-weight: 400;
    font-size: 48px;
    line-height: 59px;

    /* identical to box height */

    color: #FFFFFF;


  }
}
</style>