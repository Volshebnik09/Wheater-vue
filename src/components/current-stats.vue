<template>
  <div class="current-stats">
    <div class="current__stat">
      <img src="@/images/speed.svg" alt="" class="current__stat__icon">
      <div class="current__stat__block">
        <span class="current__stat__number">{{pressure}}</span>
        <span class="current__stat__desk">mb</span>
      </div>
    </div>
    <div class="current__stat">
      <img src="@/images/water-drop.svg" alt="" class="current__stat__icon">
      <div class="current__stat__block">
        <span class="current__stat__number">{{humidity}}</span>
        <span class="current__stat__desk">%</span>
      </div>
    </div>
    <div class="current__stat">
      <img src="@/images/wind.svg" alt="" class="current__stat__icon">
      <div class="current__stat__block">
        <span class="current__stat__number">{{windPower}}</span>
        <span class="current__stat__desk">mph, </span>
        <span class="current__stat__add">{{windDirection}}</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "current-stats",
  props: [
      'pressure',
      'humidity',
      'windPower',
      'windDirection',
  ]
}
</script>

<style scoped lang="scss">

  .current-stats {
    width: fit-content;
    margin: auto;
    display: flex;
    align-items: center;
    justify-content: center;
    gap:10px;
    padding: 0 0 -15px 0;
    margin-bottom: -15px;
    flex-wrap: wrap;
    .current__stat{
      display: flex;
      align-items: center;
      gap: 8px;
      &__number, &__add{
        font-family: 'Montserrat';
        font-style: normal;
        font-weight: 600;
        font-size: 14px;
        line-height: 15px;
        margin-right: 5px;
        /* identical to box height */

        color: #FFFFFF;
      }
      &__desk {
        font-family: 'Montserrat';
        font-style: normal;
        font-weight: 400;
        font-size: 14px;
        line-height: 15px;

        /* identical to box height */

        color: #D9D9D9;


      }
    }
  }

</style>