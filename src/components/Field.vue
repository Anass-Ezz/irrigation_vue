<template>
    <div class="field">
      <div class="led" id="led" :style= "[data.current_data.flow > 0 ? {'background': 'green'} : {'background': 'grey'}]">{{data.id}}</div>
        <div class="form-check form-switch" >
            <input @change="handleChange" class="form-check-input" v-model="data.new_data"  style="width:90px; height: 42px; margin-bottom: 10px;" type="checkbox" role="switch" id="sw" >
        </div>   
        <div id="info-wrapper" >
            <div >
                Flow
                <p class="flow">{{data.current_data.flow}} <span style="color:aqua; font-size: small;">L/min</span></p>
            </div>
            <div style="border: solid .5px grey; width: 1px; height: 80px; opacity: .4;"></div>
            <div>
              Quantity
                <p class="quantity">{{data.current_data.quantity}} <span style="color:aqua; font-size: small;">L</span></p>
            </div>
              <div style="border: solid .5px grey; width: 1px; height: 80px; opacity: .4;"></div>
            <div>
                Battery
                
                <p class="battery">{{data.current_data.battery}} <span style="color:aqua; font-size: small;">%</span></p>
            </div>
        </div>    
    </div>
  
</template>

<script>
import axios from 'axios'
export default {
  name: 'Field',
  props: {
    data: Object,
  },
  data(){
    return{
      interval:null
    }
  },
  methods: {
    handleChange: function () {
      var data_to_submit = {}
      data_to_submit[this.data.id] = this.value
      axios({
          method: 'post',
          url: 'https://anassbelkadi.pythonanywhere.com/api/post_new_data/',
          data: {
            "id":this.data.id,
            "state":this.data.new_data,
          }
      }).then(res=>{
          
      });
    },
   
  },
}
</script>

<style scoped>
.led{
      width: 100px;
      height: 100px;
      background-color: grey;
      display: flex;
      justify-content: center;
      align-items: center;
      color: white;
      margin: 10px;
      border-radius: 10px;
  }
  #info-wrapper{
      width: 100%;
      display: flex;
      justify-content: space-around;
      align-content: center;
      font-size: 20px;
      text-align: center;
      height: 40%;
  }
  /* #info-wrapper div {
    border: solid 1px red;
  } */
  .field{
    height: 50vh;
    width: 29vw;
    min-width: 350px;
    display: flex;
    align-items: center;
    justify-content: center;
    border: solid 1px white;
    border-radius: 10px;
    flex-direction: column;
  }
</style>
