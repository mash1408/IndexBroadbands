<template>
  <q-page class="flex flex-center">
   

  </q-page>
</template>

<script>
Instamojo.open("YOUR_INSTAMOJO_LINK_HERE");
export default {
  name: 'donate',
  methods:{
     map: function() {
      return this.$router.push('/')
    },
    signUp:function(){
   return this.$router.push('/signup') 
  }
  }
  
}
</script>