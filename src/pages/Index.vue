<template>
  <q-page class=" ">
    <div class="row justify-between main">
    <q-btn color="red" icon-right="send" label="Go To Map" v-on:click="map"/>
    <q-btn color="red" icon-right="send" label="Signup" v-on:click="signUp"/>
<q-btn color="red" icon-right="send" label="Send us a Coffee" v-on:click="donate"/>
<q-input filled v-model="number" label="Watsapp Number" />
 <q-btn round dense flat icon="add" />
 <a href="https://api.whatsapp.com/send?text=Hey%20there%2C%20Register%20Your%20Broadband%20COnnection%20%0Today%20And0%20Help%20Others%20out!"><img src="https://res.cloudinary.com/dxafjfxod/image/upload/v1595861934/WhatSharer/Powered_By_WhatSharer_izeybs.png" style="display: block; margin-left: auto; margin-right: auto; max-width: 200px; width: 50%;" title="Share on WhatsApp" alt="Share on WhatsApp"></a>
 
 @click="send"  />
 <!-- Features section -->
    <h2 class="text-center">Features</h2>
    <p class="text-center" style="font-size:20px">Here are some amazing features of the tracker that you will fall in love with
    </p>
    <!-- Cards -->
    <div class="Cards">
      <!-- card1 -->
      <div class="card card1">
        <h5>Mark locations</h5>
        <p>Mark the loaction along with the convinience of specifying the type of vehicle the parking is meant for</p>
        <img src="statics/undraw_Map_dark_k9pw.svg" alt="">
      </div>
      <!-- card2 -->
      <div class="card card2">
        <h5>See information</h5>
        <p>Have a Legend to help you read the map</p>
        <img src="statics/undraw_no_data_qbuo.svg" alt="">
      </div>
      <!-- card3 -->
      <div class="card card3">
        <h5>Download co-ordinates</h5>
        <p>Download the co-ordinates in the desired format i.e. CSV or GeoJSON</p>
        <img src="statics/undraw_download_files_aydf.svg" alt="">
      </div>
      <!-- card4 -->
      <div class="card card4">
        <h5>Upload co-ordinates</h5>
        <p>Have the co-ordinates with you but don't know where to mark? Upload it and get the location marked on the map</p>
        <img src="statics/app-logo-128x128.png" alt="">
      </div>
    </div>
  </div>
  </q-page>
</template>

<script>

export default {
  name: 'PageIndex',
  data(){
    return{
    number:'',
    dense:false
    }
  },
  methods:{
     map: function() {
      return this.$router.push('/')
    },
    send: function(event) {
            window.open("https://api.whatsapp.com/send?phone=xxxxxxxxxx"+this.number);
        },
      onOpenHandler () {
        alert('Payments Modal is Opened');
      },

      onCloseHandler () {
        alert('Payments Modal is Closed');
      },

    onPaymentSuccessHandler (response) {
        alert('Payment Success');
        console.log('Payment Success Response', response);
      },

    onPaymentFailureHandler (response) {
        alert('Payment Failure');
        console.log('Payment Failure Response', response);
      },
    signUp:function(){
   return this.$router.push('/signup') 
  },
  donate:function(){
    var self=this
Instamojo.open('https://www.instamojo.com/@mash1408');
Instamojo.configure({
    handlers: {
      onOpen: self.onOpenHandler(),
      onClose: self.onCloseHandler(),
      onSuccess: self.onPaymentSuccessHandler(response) ,
      onFailure: self.onPaymentFailureHandler(response)
    }
  })
  }
  }
  
}
</script>
<style scoped>
  .main {
    color: #F5F7F4;
  }

  .main-btn {
    position: relative;
    top: 25vh;
  }

  .mainp {
    width: 100%;
  }

  .overlay {
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    height: 100%;
    width: 100%;
    background: rgba(31,21,33, 0.78);
    opacity:0.9;
    text-align: center;
  }

  .about-img img {
    width: 90%;
  }

/* Styles of Features cards */
  .Cards {
    display: flex;
    flex-wrap: wrap;
    text-align: left;
    justify-content: center;
    margin-top: 50px;
  }

  .card {
    border: none;
    border-top-style:solid;
    border-top-width: 4px;
    border-radius: 9px;
    box-shadow: 0px 5px 8px 3px rgba(0,0,0, 0.1);
    width: 30%;
    max-width: 350px;
    height: auto;
    padding: 30px;
    padding-top: 5px;
    overflow: auto;
    margin: 15px;
  }

  .card img {
    width: 100px;
    height: 100px;
    float: right;
  }

  .card p {
    text-align: left;
    margin: 15px 10px 30px ;
    font-size: 13px;
  }

  .card1 {
    transform: translateY(50%);
  }  

  .card3 {
    transform: translateY(50%);
  }

  .card4 {
    border-top-color: #a58bb9;
  }

@media (max-width:900px) {
  .card {
    width: 45%;
  }
}

@media (max-width:770px) {
    .card1 {
        transform: translateY(0%);
    }
    .card3 {
        transform: translateY(0%);
    }
}

@media (max-width:650px) {
  .card {
    width: 95%;
  }
}
</style>

<style lang="stylus" scoped>
  .bg-light {
    background: $light;
  }
  .text-light {
    color: $light;
  }
  .card1 {
    border-top-color: $primary1;
  }
  .card2 {
    border-top-color: $secondary;
  }
  .card3 {
    border-top-color: $accent;
  }
  
</style>
