<template>
  <div id='app'>
    <header>
      <h1>Carpark Availability</h1>
    </header>
    <div id='container'>
      <GreetGreet v-bind:email="email" v-bind:token='token'/>
    </div>
  </div>
</template>
<script>
import GreetGreet from './Greeting';

export default {
  name: 'App',
  components: {
   GreetGreet,
  },
  mounted() {
    fetch(`http://localhost:9000/user`, {
      credentials: "include" // fetch won't send cookies unless you set credentials
    })
      .then(response => response.json())
      .then(data => {
        console.log(data,'DATA!');
        //this.email = data.user.email
        this.email=data.introspectResponse.email
        this.token=data.token
      });
    
    
  },
  data() {
    return {
      email: null,
      token: null
    }
  },
  methods:{
    tokenGetter(){
      fetch(`http://localhost:9000/user`, {
      credentials: "include" // fetch won't send cookies unless you set credentials
    })
      .then(response => response.json())
      .then(data => {
        console.log(data,'DATA!');
        
      });
    }
  }
};
</script>
<style>
@import'~bootstrap/dist/css/bootstrap.css';
h1 {
  text-align: center;
  font-size: 40px;
  font-family: Arial, Helvetica, sans-serif;
}

#container {
  box-sizing: border-box;
  border-radius: 15%;
  
  margin: auto;
}
</style>