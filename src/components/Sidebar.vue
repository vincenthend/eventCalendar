<template>
    <div class="sidebar" :toggled="toggled">
        <div class="toggle-button" v-on:click="toggled = !toggled"></div>
        <div class="overlay" :toggled="!toggled"></div>
    </div>
</template>

<script>
    export default {
        data : function(){
            return {
                toggled: true,
            }
        },
        created(){
            
        }
    }
</script>

<style>

    @media (max-width: 575px){            
        .sidebar {
            display: flex;
            flex-direction: column;
            width:100vw;
            height:15rem;
            margin-bottom:1rem;
        }

        .sidebar[toggled] {
            height: 3rem;
        }

        .toggle-button {
            cursor: pointer;
            width: 100%;
            height: 1rem;;
            display: flex;
            justify-content: center;
            padding: 1rem 0;
        }

        .sidebar[toggled] .toggle-button::before {
            content: "\f107"
        }
        .sidebar .toggle-button::before {
            content: "\f106"
        }
    }
    @media (min-width: 576px){
        .sidebar {
          display: flex;
          flex-direction: row;
          justify-content: space-between;
          width: 25rem;
          height: 100vh;
        }
        .sidebar[toggled] {
            width: 3rem;
        }
        
        .toggle-button {
            width: 1rem;
            height: 100%;
            display: flex;
            align-items: center;
            padding: 1rem;
        }

        .sidebar[toggled] .toggle-button::before {
            content: "\f105"
        }
        .sidebar .toggle-button::before {
            content: "\f104"
        }
    }

    .sidebar {
      position: fixed;
      z-index: 5;
      background-color: #FFF;
      transition-duration: 0.2s;
    }

    .toggle-button {
        font-family: "FontAwesome";
        background-color: #fff;
    }
    
    .overlay[toggled] {
        transition-duration: 0.2s;
        z-index: -1;
        position: fixed;
        opacity:0.2;
        background-color: #999;
        width: 100vw;
        height: 100vh;
    }
</style>
