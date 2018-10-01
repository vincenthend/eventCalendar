<template>
    <div class="datebox">
        <div class="dateStack">
            <div class="day">
                {{ dateInterfaced[0] }}
            </div>
            <div class="date">
                {{ dateInterfaced[1] }}
            </div>
            <div class="month">
                {{ dateInterfaced[2] }}
            </div>
            <div class="year">
                {{ dateInterfaced[3] }}
            </div>
        </div>
        <div class="events">
            <Event @selectEvent="handleEventClick" v-for="event in this.events" :key="event.id" :event="event"></event>
        </div>
    </div>
</template>

<script>
    import Event from './Event.vue'

    export default {
        data : function(){
            return {
                dateInterfaced : ["Senin", 1, "Januari", 1970],
                month : ["Januari", "Februari", "Maret", "April", "Mei", "Juni", "Juli", "Agustus", "September", "Oktober", "November", "Desember"],
                day : ["Minggu", "Senin", "Selasa", "Rabu", "Kamis", "Jumat", "Sabtu"],
            }
        },
        props: {
            date : {
                type: Number,
                required: true
            },
            events : {
                type: Array
            }
        },
        components: {
            Event
        },
        methods: {
            handleEventClick : function(event){
                this.$emit('selectEvent', event)
            }
        },
        created(){
            var dateObj = new Date(this.date)
            this.dateInterfaced = [this.day[dateObj.getDay()], dateObj.getDate(), this.month[dateObj.getMonth()], dateObj.getYear()+1900]
        }
    }
</script>

<style scoped>
    @media (max-width: 575px){
        .datebox {        
            margin-left: 1rem;
            margin-right: 0.5rem;
            margin-top: 1rem;
        }
    }
    @media (min-width: 576px){
        .datebox {
            margin-top: 1rem;
            min-width: 40rem;
            margin-left: 0.5rem;
            margin-right: 0.5rem;
        }
    }

    .datebox {    
        display: flex;
        flex-direction: row;
        background-color: #fff;
        
        overflow: hidden;
        border-radius: 10px;
    }

    .dateStack {
        text-align: center;
        font-weight: bold;
        color : #FFF;
        background-color: #039BE5;
        border-bottom: none;
        
        display: flex;
        flex-direction: column;
        
        padding : 0.5rem 1rem 1rem 1rem;
    }

    .dateStack .month {
        font-size: 1rem;
    }

    .dateStack .date {
        margin: -0.4rem 0;
        font-size: 3.4rem;
    }

    .dateStack .year {
        font-size: 0.8rem;
    }
</style>
