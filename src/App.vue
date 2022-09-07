<template>
  <div class="title">IRRIGATION DASHBOARD</div>
  <div class="wrapper">
    <Field v-for="node in nodes" :data="{id:node, current_data:current_data[node], new_data:new_data[node]}"/>
  </div>
  
</template>

<script>
import Field from './components/Field.vue'
import axios from 'axios'
export default {
  name: 'App',
  components: {
    Field
  },
  data(){
    return{
      nodes : [],
      current_data : {},
      new_data : {},
      interval: null,
    }
  },
  methods: {
    loadCurrentData: function () {
      axios.get("https://anassbelkadi.pythonanywhere.com/api/get_current_data/").then((res)=>{
        this.nodes = res.data.nodes
        this.current_data = res.data

      })
    },
     loadNewData: function () {
      axios.get("https://anassbelkadi.pythonanywhere.com/api/get_new_data/").then((res)=>{
        console.log(this.new_data)
        this.new_data = res.data
      })
    }
  },
  created: function () {
    this.loadCurrentData();
    this.loadNewData()
    this.interval = setInterval(function () {
      this.loadCurrentData();
      this.loadNewData()
    }.bind(this), 1000); 
  },
  beforeDestroy: function(){
    clearInterval(this.interval);
  }
}
</script>

<style>
  body{
    background-color: rgb(30, 30, 30);
    color: white;
  }
  .title{
    font-size: 50px;
    text-align: center;
  }
  .wrapper{
    display: flex;
    gap: 20px;
    justify-content: center;
    margin: 20px;
    flex-wrap: wrap;
    
  }
  
</style>
