<template>
      
      <!-- <MainPage/> -->
      <ByePage/>
</template>

<script>
import MainPage from './components/MainPage.vue'
import ByePage from './components/ByePage.vue'
export default {
  name: 'App',
  components: {
    MainPage,
    ByePage
  },
  data() {
    return {
      loading: true
    };
  },
  created() {  
    setTimeout(() => {
      this.loading = false;
    }, 1000);
  },
  
  methods:{
    startLoading(){
      this.loading = true
    },
    finishLoading(){
      this.loading = false
    }
  }
}
</script>

