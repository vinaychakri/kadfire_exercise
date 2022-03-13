
<!--

Please complete the whole exercise in this file using only HTML, CSS and vanilla Javascript

Make a call to https://fakestoreapi.com/products which is a free API,
    the API can be very slow, so show a loading page until the data is retrieved.

Once the data is retrieved start a loop which displays a clock with top timer for five seconds,
followed by the first item from the shop API for five seconds,
then clock for five seconds,
then second shop item etc endless loop.

I am not so concerned about the look of the data, more in getting the data and smooth
 transition between the screens.

Our screen resolution is usually 1080x1920, so work with these dimensions. 
I recommend developing with device toolbar set to these dimensions, as in the image, 
to emulate the kiosk mode we run in.

Please refer to the example mp4 provided, any question, drop me an email at

richard.ganderton-smith@kadfire.com

-->




<template>
<div class="">
    <div class="item-wrapper">
    <div class="loading-data">
      <!-- <ul v-if="list.length > indexVariable"> -->
        <!-- <li v-for="item in indexVariable" :key="item"> -->

          <p v-if="!list || !list.length">Loading Data ...?</p>
          <h1 class="clock-wrapper" v-else-if="timerCount !=0">
            <div class="loading-bar">
            <div class="percentage" :style="{'width' : percentage + '%'}"></div>
            </div>
            <div class="time-display">{{time}}</div>
          </h1>
            <h6 class = "data-wrapper" v-if=" list.length >= indexVariable && timerCount === 0">
              Item id: {{list[indexVariable].id}}<br>
              Title :{{list[indexVariable].title}}<br>
              Price: {{list[indexVariable].price}}<br>
              Description:{{list[indexVariable].description}}
          </h6>
          <!-- {{item++}} -->
        <!-- </li> -->
      <!-- </ul> -->
          
    </div>

  </div>
</div>
</template>
  <script>
  import axios from 'axios';
  export default {
    name: "KadfireExercise",    
    data(){
      return {
        list: [],
        interval: null,
        time: null,
        timerCount: 5,
        indexVariable: 0,
        percentage: 0

      };
    },
    watch:{
      timerCount:{
        handler(value){
          if(value > 0){

            setTimeout(()=>{
              this.timerCount--;
            },1000);
            return this.time
          }
        },
        immediate: true
      },
    },

    computed: {
    percent() {
      return this.percentage.toFixed();
    }
  },
    beforeDestroy() {
    // prevent memory leak
    clearInterval(this.interval)
    },
    created(){
      this.interval = setInterval(()=>{
       this.time = Intl.DateTimeFormat(navigator.language, {
        hour: 'numeric',
        minute: 'numeric',
        second: 'numeric'
      },1000).format() 
      });
      var percentIntervel = setInterval(()=>{
        if(this.percentage < 100)
        this.percentage += 0.21;
      else
        clearInterval(percentIntervel);
      },10)
    },


    async mounted(){
      let result = await axios.get("https://fakestoreapi.com/products");
    console.warn("data recievied from api",result.data);
    this.list = result.data;
    },

  }
  
  </script>

  <style>
    body {
      font-family: Arial, Helvetica, sans-serif;
      height: 1920px;
      width: 1080px;
      padding: 10px;
      margin: 0;  
      color: #fff;
      background-color: #000;
      border: 10px solid #fff;
      overflow: auto;
        box-sizing: border-box;


    }
    .clock-wrapper {
      font-size: 45px;
      position: absolute;
      color: #fff;
      background-color: #000;
      opacity: 1;
      z-index: -1;
    }
    .data-wrapper{
      height: 50%;
      width: 20%;
    }
    .loading-bar {
  position: relative;
  width: 250px;
  height: 10px;
  border-radius: 10px;
  overflow: hidden;
  border-bottom: 1px solid #ddd;
    }
  .percentage {
    position: absolute;
    display: block;
    height: 15%;
    border-radius: 15px;
    background-color: #c9d6b2;
    
}
.time-display{
    padding-top: 60%;
    padding-left: 10%;
}
  </style>

