<template>
    <div id="app">
            <sidebar></sidebar>
            <div class="status" v-if="!loaded"><div>{{this.status}}</div></div>
            <div class="workarea" v-if="loaded">
                <DateBox @selectEvent="previewEvents" v-for="data in caldata" :key="data.id" :date="data.date" :events="data.event_list"></datebox>
            </div>
            <Preview :toggled="preview_toggle" :event="this.selected_data"></Preview>
    </div>
</template>

<script>
import DateBox from './components/DateBox.vue'
import Sidebar from './components/Sidebar.vue'
import Preview from './components/Preview.vue'

export default {
    components: {
        DateBox, Sidebar, Preview
    },
    data: function() {
      return {
        status : "Loading",
        loaded : false,
        endpoint : 'https://script.google.com/macros/s/AKfycbw5wP7kczmWpK-dP4GCzrfpA1N76m6r7c5rhqxVt8jOEkDseqY/exec?method=getEventByDate&param=',
        caldata : [],
        toggled : true,

        selected_data : {},
        preview_toggle : true,
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
        });
      },
      previewEvents : function(event){
        this.preview_toggle = false;
        this.selected_data = event;
      }
    },
    created(){
      this.loadEvents()
    }
}
</script>

<style>
  @font-face {
      font-family: "FontAwesome";
      src: url("./assets/fonts/fa-solid-900.woff") format('woff');
  }

  @font-face {
      font-family: "Product Sans";
      src: url('./assets/fonts/Product Sans Regular.ttf');
  }

  * {
      font-family: "Product Sans", sans-serif;
  }

  body {
      background-color: #EFEFEF;
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
        margin-left: 3rem;
        padding-left: 1rem;
      }
  }
</style>
