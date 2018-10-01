<template>
    <div class="event" @click="selectEvent">
        <div class="time" v-if="!allDay">{{start}} - {{end}}</div>
        <div class="event-name">{{name}}</div>
    </div>
</template>

<script>
    export default {
        data: function(){
            return {
                start : "00:00",
                end : "00:00",
                name : "Nama",
                allDay : false,
            }
        },
        props: {
           event : {
                type: Object,
                required: true
            }
        },
        methods : {
            selectEvent : function(event){
                this.$emit('selectEvent', this.event)
            }
        },
        created(){
            var start = new Date(this.event.start*1000);
            var end = new Date(this.event.end*1000);

            this.allDay = this.event.allDay;
            this.start = start.getHours() + ":" + start.getMinutes();
            this.end = end.getHours() + ":" + end.getMinutes();
            this.name = this.event.name;
        }
    }
</script>

<style>
    .events {
        font-size: 0.9rem;
        padding: 0.2rem;
        padding-left: 0;
    }

    .event {        
        cursor: pointer;     
        display: flex;
        flex-direction: row;
        padding: 0.3rem;
        color: #fff;
        background-color: #039BE5;
        border-radius: 0.3rem;
        margin: 0.2rem 0.2rem;
    }

    .event .time {
        margin-right: 0.5rem;
    }
</style>