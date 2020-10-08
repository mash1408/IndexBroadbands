<template>
  <q-page class="flex flex-center">
    <q-btn color="red" icon-right="send" label="Go To Map" v-on:click="map"/>
  </q-page>
</template>

<script>
export default {
  name: 'PageIndex',
  methods:{
     map: function() {
      return this.$router.push('/')
    }
  }
}
</script>
