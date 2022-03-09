<template>
  <div class="greet">
    <div v-if="email">
    <h3>
        Welcome {{email}} 
    </h3>
        your token is 
        <div class='token'>{{token}}</div>

       <div>
           <h5>Details:</h5>
           First name:<br>
           Last name:<br>
           Email: {{email}}<br>
           Contact:<br>

           <button @click='carpark'>Carpark Availability</button>
  </div>
  <div class='container'>
    <div class='row justify-content-center'>
  <div class='col-3 m-1 bg-light' v-for='a in data' v-bind:key="a">
    Number: {{a['number']}} <br>
    <p style='text-decoration:bold'>
    Type: {{a['type']}} <br>
    Available: {{a['available']}} <br>
    Total: {{a['total']}} <br>
    </p>
    Last Updated: <br>{{a['updated']}}
    
      </div>
      </div></div>
    </div>
    <h3 v-else>You are not logged in</h3>
  </div>
</template>
<script>
export default {
  name: 'GreetGreet',
  props: ["email",'token'],
  data(){
      return({
          data:[]
      })
  },
  methods:{
      async carpark(){
         let a = this
         await fetch('https://api.data.gov.sg/v1/transport/carpark-availability')
            .then(function(response) {
                // The response is a Response instance.
                // You parse the data into a useable format using `.json()`
                return response.json();
            }).then(function(data) {
                // `data` is the parsed version of the JSON returned from the above endpoint.
                let carparkDataArray;
                let timestamp;
                //console.log(data['items'][0],'data here');  
                carparkDataArray = data['items'][0]['carpark_data']
                timestamp = data['items'][0]['timestamp']

                console.log(carparkDataArray)
                console.log(timestamp,'timestamp')
                
                for (let element of carparkDataArray){
                  
                    let carparkData = {}
                    carparkData['number']=element['carpark_number']
                    carparkData['type']=element['carpark_info'][0]['lot_type']
                    carparkData['available']=element['carpark_info'][0]['lots_available']
                    carparkData['total']=element['carpark_info'][0]['total_lots']
                    carparkData['updated']=element['update_datetime']
                    a.data.push(carparkData)
                    //console.log(carparkData,'DATA!!!!!')
                   
                }
                
                
            });
        console.log(a.data)
      }
  }
};
</script>
<style>
.token{
    
    word-wrap: break-word
}
* {
  margin-top: 30px;
  text-align: center;
  font-size: 20px;
  font-family: 'Courier New', Courier, monospace;
}
</style>