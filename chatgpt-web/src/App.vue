<template>

      <MainPage/>

</template>

<script>
import MainPage from './components/MainPage.vue'

export default {
  name: 'App',
  components: {
    MainPage
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

