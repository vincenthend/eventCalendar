<template>
    <div id="app">
            <div class="sidebar" toggled>
                
            </div>
            <div class="status" v-if="!loaded"><div>{{this.status}}</div></div>
            <div class="workarea" v-if="loaded">
                <DateBox v-for="data in caldata" :key="data.id" :date="data.date" :events="data.event_list"></datebox>
            </div>
            <div class="previewPane">
            </div>
    </div>
</template>

<script>
import DateBox from './components/DateBox.vue'

export default {
    components: {
        DateBox
    },
    data: function() {
      return {
        status : "Loading",
        loaded : false,
        endpoint : 'https://script.google.com/macros/s/AKfycbw5wP7kczmWpK-dP4GCzrfpA1N76m6r7c5rhqxVt8jOEkDseqY/exec?method=getEventByDate&param=',
        caldata : []
      }
    },
    methods: {
      loadEvents : function(){
        this.loaded = false;
        
        var url = this.endpoint + JSON.stringify({date:parseInt(new Date().getTime()/1000), duration:7}) + "&callback=?";
        fetch(url).then(res => res.json()).then(res => {
          this.loaded = true;
          if (res.status = true){
            this.caldata = res.events;
          }
          console.log(res)
        });
      }
    },
    mounted(){
      this.loadEvents()
    }
}
</script>

<style>
  @font-face {
      font-family: "Product Sans";
      src: url('../fonts/Product Sans Regular.ttf');
  }

  * {
      font-family: "Product Sans", sans-serif;
  }

  body {
      background-color: #ddd;
      margin: 0;
  }

  .status {
    display: flex;
    align-items: center;
    justify-content: center;
    
    width: 90vw;
    height: 100vh;
  }
  .status div {
    font-size: 2rem;
    color: #999;
  }

  @media (max-width: 575px){   
      #app {
          display: flex;
          flex-direction: column;
          overflow: visible;
      }
      .sidebar {
          width:100vw;
          height:15rem;
          margin-bottom:1rem;
      }

      .sidebar[toggled] {
          height: 3rem;
      }
      .sidebar ~ .workarea {
          margin-top: 3rem;
      }
  }
  @media (min-width: 576px){    
      #app {
          display: flex;
          flex-direction: row;
          overflow: visible;
      }
      .workarea {
          padding-left: 1rem;
      }

      .sidebar {
          width: 25rem;
          height: 100vh;
      }
      .sidebar[toggled] {
          width: 3rem;
      }
      .sidebar[toggled] ~ .workarea {
          margin-left: 3rem;
      }
  }

  .sidebar {
      position: fixed;
      z-index: 1;
      background-color: #FFF;
      transition-duration: 0.2s;
  }
</style>
