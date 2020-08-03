<template>
    <div class="calendar" id="wrapper">
        <div class="calendar-header">
            <a href="#" @click="subtractMonth">&#10096;&#10096;&#10096;</a>
            <h4>{{month + ' - ' + year}}</h4>
            <a href="#" @click="addMonth">&#10097;&#10097;&#10097;</a>
        </div>

        <div class="weekdays" v-for="(day,index) in days" :key="day+'index'+index">
            {{ day }}
        </div>
        <!-- <ul class="weekdays">
            <li v-for="(day,index) in days" v-bind:key="day+index">{{ day }}</li>
        </ul> -->

        <div class="dates" id="cal">
            <div v-for="(blank,index) in firstDayOfMonth" :key="index+'blank'" class="cal-square">
               <span>&nbsp;</span>
            </div>
            
            <template v-for="(date,index) in daysInMonth">
                <div v-if="[6,13,20,27,34].includes(index+firstDayOfMonth)" class="cal-square calSaturday" :key="index+'date'" :class="{'current-day': date == initialDate &amp;&amp; month == initialMonth && year == initialYear}">
                    <span> {{ date }} </span>
                </div>

                <div v-else :key="index+'date'" class="cal-square" :class="{'current-day': date == initialDate &amp;&amp; month == initialMonth && year == initialYear}">
                    <span>{{ date }}</span>
                </div>

            </template>
        </div>
    </div>
</template>

<script>
import moment from 'moment'

export default {

    data() {
        return {
            today: moment(),
            dateContext: moment(),
            days: ['S','M','T','W','R','F','S']
        }
    },

    computed: {
        year: function() {
            var t = this;
            return t.dateContext.format('Y');
        },
        month: function() {
            var t = this;
            return t.dateContext.format('MMMM');
        },
        daysInMonth: function() {
            var t = this;
            console.log(typeof(t.dateContext.daysInMonth()))
            return t.dateContext.daysInMonth();
        },
        currentDate: function() {
            var t = this;
            return t.dateContext.get('date');
        },
        firstDayOfMonth: function() {
            var t = this;
            var firstDay = moment(t.dateContext).subtract((t.currentDate-1),'days');
            return firstDay.weekday();
        },
        initialDate: function() {
            var t = this;
            return t.today.get('date');
        },
        initialMonth: function() {
            var t = this;
            return t.today.format('MMMM');
        },
        initialYear: function() {
            var t = this;
            return t.today.format('Y');
        }
    },

    methods: {
        addMonth: function() {
            var t = this;
            t.dateContext = moment(t.dateContext).add(1,'month');
        },
        subtractMonth: function () {
            var t = this;
            t.dateContext = moment(t.dateContext).subtract(1,'month');
        }
    }

}
</script>

<style scoped>

div {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

#wrapper {
    text-align: left;
    padding: 5% 0 0 30%;
}

#cal {
    text-align: left;
    
}

.calendar-header h4 {
    color: #000000;
    display: inline;
    vertical-align: middle;
}

.calendar-header a {
    padding: 0 25px 0 25px;
    text-decoration: none;
    font-weight: 600;
    color: #6f6f6f;
}

.calendar-header a:hover {
    color: blue;
}

.weekdays {
    background-color: black;
    color: white;
    display: inline-block;
    width: 45px;
}

.weekdays li {
    float: left;
}

.blank_dates {
    list-style-type: none;
    margin: 0;
    padding: 0;
    overflow: hidden;
}

.blank_dates li {
    float: left;
}

.dates {
    list-style-type: none;
    margin: 0;
    padding: 0;
    overflow: hidden;
}

.dates li {
    float: left;
    padding: 0 15px 0 0;
}

.current-day {
    background-color: #ff0000;
}

.cal-square {
    display: inline;
}

.cal-square span {
    display: inline-block;
    width: 45px;
    height: 45px;
    text-align: left;
}

.calSaturday {
    display: inline;
}

.calSaturday span {
    display: inline-block;
    text-align: left;
    width: 45px;
    height: 45px;
}

.calSaturday::after {
    content: '\A';
    white-space: pre-wrap;
}

.current-day {
    background-color:#6f6f6f;
    color: #ffffff
}
</style>