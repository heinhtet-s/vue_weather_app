<template>
<div>
  <div id="app" :class="typeof weather.main != 'undefined'&&weather.main.temp> 16 ? 'warm':'' " >
    
    <input type="text"  autocomplete="off" class="search" id="search" placeholder="Search..." v-model="city" @keyup.enter="fetchData" @keyup="show">
    <div class="autocomplete" >
<div  style="margin-bottom: 10px;margin-top: 10px; line-height: 20px;" v-for="(item,index) in autoCompleteList" :key="index"  :id="index" @click="auto(index)">
  {{item}}
   <hr class="color: black;">
</div>
      
    </div>
    <div class="weather_box" v-if="typeof weather.main != 'undefined' " >
      <h2 style="margin-top: 20px; margin-bottom:10px;">{{weather.name}} {{weather.sys.country}}</h2>
      <p style=" color: rgba(245, 238, 238, 0.431);margin-bottom: 20px;">{{dates()}}</p>
      <div class="temperature">
        {{Math.round(weather.main.temp)}}°c
      </div>
      <h1 class="condition">{{weather.weather[0].main}} </h1>
    </div>
   
  </div>
  
  </div>
</template>

<script>


import axios from "axios";
export default {

data(){
  return {
   api_key: "fa7ef679580040d3aa8f367939740ea6",
   url_base: "https://api.openweathermap.org/data/2.5/weather",
   city: "",
   weather: {},
   autoComplete: [],
   autoCompleteList: '',
   error: '',
    }
  },
created(){
  axios.get(' https://countriesnow.space/api/v0.1/countries').then(res =>{
    res.data.data.forEach(element => {
      
      this.autoComplete.push(...element.cities);
    });
  });
},
  methods: {
  fetchData(){
      
     
 
        axios.get(`${this.url_base}?q=${this.city}&units=metric&APPID=${this.api_key}`)
        .then(res => {
             this.weather=res.data;
           this.autoCompleteList='';
           this.error='';
        }).catch( (error) =>{
          if (error.response) {
         this.error=error.response.data.message;
        this.autoCompleteList='';
         alert(this.error);
        
        }});
    
  },
  auto(id){
    let content=document.getElementById(id).textContent;
    this.city=content;
       this.fetchData();
       this.autoCompleteList='';
  },
  show(){
    this.autoCompleteList='';
    if(this.city !=''){
      this.city=this.city[0].toUpperCase() + this.city.substring(1).toLowerCase();
           
          this.autoCompleteList=this.autoComplete.filter((element)=>{
           return element.includes(this.city);
          })
         
        }      
      }
  
  ,
  
  dates(){
    let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
    let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
    let date=new Date();
    let day=days[date.getDay()];
    let month=months[date.getMonth()];
    let year=date.getFullYear();
    
    return `${day},  ${month} ,${year} `;

  }
  }

}

</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  }
  #app{
     background-image:linear-gradient(to bottom, rgba(0, 0, 0, 0.15), rgba(0, 0, 0, 0.75)),url('./assets/cold-bg.jpg');
    background-size: cover;
    background-position: bottom;
    transition: 0.4s;
    height: 100vh;

text-align: center;
padding: 25px;
  }
  .error{
   background-color: red;
   border-radius: 10px;
   width: 80vw;
   margin: auto;
   padding: 20px;
  }
  .autocomplete{
    width: 80vw;
    background-color: rgba(225, 225, 225, 0.5);
    max-height: 50vh;
    overflow: scroll;
margin:auto;

  }

  .warm{
        background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.15), rgba(0, 0, 0, 0.75)),url("./assets/warm-bg.jpg") !important;
  }
  .search{
    border: none;
          width: 80vw;
          height: 35px;
          text-decoration: none;
          padding: 5px;
          border-radius: 10px 0px;
          transition: 0.4s;
          background-color: rgba(225, 225, 225, 0.5);
          box-shadow: 0px 0px 20px rgba(0, 0, 0,0.2);
    }
    .search:focus{
     outline: none;
         border-radius: 0px 10px;
         background-color: rgba(255, 255, 255, 0.75);
      }
      .weather_box{
        color: white;
        
      }
     

      
      .temperature{
          width: 170px;
          background-color:rgba(255, 255, 255, 0.25);
          height: 90px;
          margin: auto;
          font-weight: bolder;
            border-radius: 10px;
       
          font-size: 79px;
          box-shadow: 3px 4px rgba(0, 0, 0, 0.25);
           text-shadow: 3px 4px rgba(0, 0, 0, 0.25);
        }
           .condition{
            text-shadow: 3px 4px rgba(0, 0, 0, 0.25);
           }
</style>
