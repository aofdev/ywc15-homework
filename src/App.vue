<template>
  <v-app id="app" standalone>
    <main>
      <v-container>
        <br><br>
           <transition name="slide" mode="out-in">
        <router-view :getMajor="getMajor" :content="content" :design="design" :marketing="marketing" :programming="programming" :majorAll="majorAll"></router-view>
      </transition>
        <br><br><br>
      </v-container>
      <v-snackbar :timeout="timeout" top="top" :right="null === 'right'" :left="null === 'left'" v-model="snackbar">offline mode<v-btn flat color="pink" @click.native="snackbar = false">Close</v-btn></v-snackbar>
    </main>
    <bottomNav @dataMajor="getDataMajor"></bottomNav>
  </v-app>
</template>

<script>
  export default {
    data() {
      return {
        e2: '',
        snackbar: false,
        timeout: 60000,
        getMajor: '',
        content: [],
        design: [],
        marketing: [],
        programming: [],
        majorAll: []
      }
    },
    methods: {
      getDataMajor(data) {
        this.getMajor = data
      }
    },
    beforeCreate() {
      fetch('https://ywc15.ywc.in.th/api/interview').then(response => response.json()).then(data => {
        data.forEach(element => {
          this.majorAll.push(element)
          if (element.major == 'content') {
            this.content.push(element)
          } else if (element.major == 'design') {
            this.design.push(element)
          } else if (element.major == 'marketing') {
            this.marketing.push(element)
          } else if (element.major == 'programming') {
            this.programming.push(element)
          }
        })
      })
    },
    created() {
      const vm = this;
      window.addEventListener('offline', function() { vm.snackbar = true });
      window.addEventListener('online', function() { vm.snackbar = false});
    },
    components: { 'bottomNav': () => import ('./components/bottomNav.vue') }
  }
</script>
<style>
.slide-enter-active{
    animation: slide-in 100ms ease-out forwards;
  }
  .slide-leave-active{
    animation: slide-out 100ms ease-out forwards;
  }
  @keyframes slide-in {
    from{
      transform: translateY(-30px);
      opacity: 0;
    }
    to{
      transform: translateY(0);
      opacity: 1;
    }
  }
  @keyframes slide-out {
    from{
      transform: translateY(0);
      opacity: 1;
    }
    to{
      transform: translateY(-30px);
      opacity: 0;
    }
  }
/* @import url('https://fonts.googleapis.com/css?family=Kanit');
body {
  font-family: 'Kanit', sans-serif;

} */
</style>




