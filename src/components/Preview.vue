<template>
    <div class="preview" :hide="this.toggle_data">
        <div class="preview-header">
            <div class="fa preview-close" v-on:click="togglePreview"></div>        
            <div class="preview-name"> {{event.name}} </div>
        </div>
        <div class="preview-contents">
            <div class="preview-location" v-if="event.location != ''"> <span class="fa">&#xf3c5;</span> {{event.location}} </div>
            <div class="preview-time"> <span class="fa">&#xf017;</span> {{setTime(event.start, event.end)}} </div>
            <div class="preview-desc" v-if="event.desc != ''"> <span class="fa">&#xf036;</span> {{event.desc}} </div>
        </div>
    </div>
</template>

<script>
    export default {
        data : function(){
            return {
                toggle_data : true,                
                month : ["Januari", "Februari", "Maret", "April", "Mei", "Juni", "Juli", "Agustus", "September", "Oktober", "November", "Desember"],
                day : ["Minggu", "Senin", "Selasa", "Rabu", "Kamis", "Jumat", "Sabtu"],
            }
        },
        props: {
            event : {
                type : Object,
                default : function () {
                    return {
                        name : "Event Name",
                        location : "Location",
                        time : "07.00 - 08.00",
                        desc : "Event descriptions",
                    }
                }
            },
            toggled : {
                type : Boolean
            }
        },
        components: {
            
        },
        methods:{
            togglePreview : function(){
                this.$parent.preview_toggle = true;
                this.toggle_data = true;
            },
            setTime : function(){
                var startstring;
                var endstring;

                if (this.event.allDay){
                    var start = new Date(this.event.start*1000);
                    var end = new Date(this.event.end*1000);

                    startstring = start.getDate() + " " + this.month[start.getMonth()] + " " + (start.getYear()+1900);
                    endstring =  end.getDate() + " " + this.month[end.getMonth()] + " " + (end.getYear()+1900);
                } else {
                    var start = new Date(this.event.start*1000);
                    var end = new Date(this.event.end*1000);

                    var startdatestring = start.getDate() + " " + this.month[start.getMonth()] + " " + (start.getYear()+1900);
                    startstring = startdatestring + ", " + start.getHours() + ":" + start.getMinutes();
                    endstring = end.getHours() + ":" + end.getMinutes();
                }                

                return startstring + " - " + endstring;
            }
        },
        watch : {
            toggled : function(){
                this.toggle_data = this.toggled;
            },
            event : function(){
                this.toggle_data = this.toggled;
            }
        }
    }
</script>

<style>
    @media (max-width: 575px){
        .preview {
            bottom: 0rem;
            width: 100vw;
            overflow: hidden;

            -webkit-box-shadow: 0px -8px 11px -2px rgba(97,97,97,0.46);
            -moz-box-shadow: 0px -8px 11px -2px rgba(97,97,97,0.46);
            box-shadow: 0px -8px 11px -2px rgba(97,97,97,0.46);

            border-top-left-radius: 10px;
            border-top-right-radius: 10px;
            
            transition-duration: 0.5s;
        }
        
        .preview[hide]{
            bottom: -30rem;
        }
        .preview-name {
            margin-top: 1rem;
        }
        .preview-name {
            font-size: 1.7rem;
            padding: 1rem;
        }
    }
    @media (min-width: 576px){
        .preview {    
            height: 100vh;
            width: 25rem;
            right: 0rem;
            transition-duration: 0.5s;
            -webkit-box-shadow: -8px 0px 11px -2px rgba(97,97,97,0.46);
            -moz-box-shadow: -8px 0px 11px -2px rgba(97,97,97,0.46);
            box-shadow: -8px 0px 11px -2px rgba(97,97,97,0.46);
        }
        .preview-name {
            font-size: 2rem;
            padding: 1rem;
            margin-top: 4rem;
        }
        .preview[hide]{
            right: -30rem;
        }
    }

    .preview {  
        position: fixed;
        z-index: 5;
        background-color: #FFF;
        transition-duration: 0.2s;
    }

    .fa{
        font-family : "FontAwesome"
    }

    .preview-header {        
        padding: 1rem;
        padding-bottom: 0;
        color: #FFF;
        background-color: #039BE5;
    }

    .preview-close {
        cursor: pointer;        
        text-align: right;
    }
    .preview-close::before {
        content : "\f00d"
    }

    .preview-contents {
        padding: 1rem;
    }
    
    .preview-location, .preview-time, .preview-desc {
        margin-bottom: 0.3rem;
    }
</style>