<template>
  <div>
    <weatherApi :apiKey="apiKey" v-model="selectedCity" />
  </div>
</template>

<script>
import weatherApi from '@/components/weather-api.vue';

export default {
  components: {
    weatherApi
  },
  data(){
    return{
      apiKey: "b0ba3a4b2f6555225b4b5a85bf483676"
    }
  }
}
</script>