<template>
  <div class="top">
    <form class="search" action="GET" ref="form" autocomplete="off">
      <img src="@/images/bx_search.svg" alt="" class="search-icon" @click="sendRequest" >
      <div class="search__input">
        <input v-model="searchQuery" name="search" placeholder="Адрес...">
        <div class="search__input__modify__">
          <a v-for="item in searchRequestResult"
             v-bind:href="'?search=' + item.url"
             class="search__input__modify"
             :key="item.id"
          >
            {{item.name}}
          </a>
        </div>
      </div>
    </form>
    <img src="@/images/bx_cog.svg" alt="" class="settings">
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "top",
  props: [
    "searchQuery",
  ],
  data(){
    return{
      searchQueryEditSettings: 100,
      searchQueryEdit: this.searchQueryEditSettings,
      sendSearchRequest: false,
      searchRequestResult: {},
    }
  },
  methods:{
    sendRequest() {
      this.$refs.form.submit()

    }
  },
  watch:{
    searchQuery: function(val){
      if (val.length < 3) return
      if (this.searchQueryEdit >= this.searchQueryEditSettings) {
        let decrease = ()=> {
          this.searchQueryEdit--

          if (this.searchQueryEdit > this.searchQueryEditSettings-1) {

            return
          }
          if (this.searchQueryEdit > 1) {
            console.log(this.searchQueryEdit)
            if (!Object.keys(this.searchRequestResult).length == 0) {
              this.searchRequestResult = {}
            }
            setTimeout(()=>{decrease()},1)
            return
          }
          if (this.searchQueryEdit === 1) {
            console.log('allowRequest')
            this.sendSearchRequest = true
          }
        }

        decrease()
      } else {
        this.searchQueryEdit = this.searchQueryEditSettings
      }
    },

    sendSearchRequest: function (val){
      if (this.searchQuery.length < 3) {
        this.sendSearchRequest = false;
        return
      }
      if (this.sendSearchRequest) {
        axios
            .get('http://api.weatherapi.com/v1/search.json?key=af2cf1a57a5b486b8b7173931221007&q='+this.searchQuery)
            .then( response => {
              console.log(response)
              this.searchRequestResult = response.data.slice(0,6)
            })
            .catch(error=>{
              console.log(error)
            })
            .finally(()=>{
              this.sendSearchRequest = false;
            })

      }
    }
  }
}
</script>

<style scoped lang="scss">

.top {
  padding: 10px 10px;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  .search {
    img {
      cursor: pointer;
    }
    display: flex;
    align-items: center;
    .search__input {
      position: relative;
      .search__input__modify__{
        height: fit-content;
        display: flex;
        flex-direction: column;
        position: absolute;
        width: 100%;
        background: #ffffff;
        border-radius: 2px;
        box-shadow: black 2px 2px 10px;
        .search__input__modify {
          cursor: pointer;
          padding: 2px;
          max-width: 400px;
          font-family: 'Montserrat';
          font-style: normal;
          font-weight: 500;
          font-size: 16px;
          line-height: 20px;
          color:black;
          text-decoration: none;

        }
        .search__input__modify:focus {
          background: gray;
          outline: none;
        }
        .search__input__modify + .search__input__modify  {
          border-top:1px solid #2f2f2f
        }
      }
    }
    input {
      background: transparent;
      border: none;
      outline: none;
      width: calc(100vw - 20px - 60px);

      max-width: 400px;
      font-family: 'Montserrat';
      font-style: normal;
      font-weight: 500;
      font-size: 16px;
      line-height: 20px;

      /* identical to box height */

      color: #FFFFFF;
    }
  }
}
</style>