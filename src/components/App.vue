<template>
    <div>
        <div id="header">
            <div>
                <h1>Vue.js Calendar</h1>
            </div>
            <div>
                <CurrentMonth />
            </div>
        </div>
        <div id="day-bar">
            <div>Mon</div>
            <div>Tue</div>
            <div>Wed</div>
            <div>Thu</div>
            <div>Fri</div>
            <div>Sat</div>
            <div>Sun</div>
        </div>
        <div id="calendar">
            <div v-for="week in weeks" :key="week" class="calendar-week">
                <CalendarDay v-for="day in week" :key="day" :day="day" />
            </div>
        </div>
        <EventForm />
    </div>
</template>
<script>
import CalendarDay from "./CalendarDay.vue";
import CurrentMonth from "./CurrentMonth.vue";
import EventForm from "./EventForm.vue";

export default {
    computed: {
        month() {
            return this.$store.state.currentMonth;
        },
        year() {
            return this.$store.state.currentYear;
        },
        days() {
            let days = [];
            let currentDay = this.$moment(`${this.year}-${this.month}-1`, "YYYY-M-D");

            do {
                days.push(currentDay);
                currentDay = this.$moment(currentDay).add(1, "days");
            } while (currentDay.month() + 1 === this.month);

            const Sunday = 0;
            const Monday = 1;

            currentDay = this.$moment(days[0]);
            if (currentDay.day() !== Monday) {
                do {
                    currentDay = this.$moment(currentDay).subtract(1, "days");
                    days.unshift(currentDay);
                } while (currentDay.day() !== Monday);
            }

            currentDay = this.$moment(days[days.length - 1]);
            if (currentDay.day() !== Sunday) {
                do {
                    currentDay = this.$moment(currentDay).add(1, "days");
                    days.push(currentDay);
                } while (currentDay.day() !== Sunday);
            }

            return days;
        },
        weeks() {
            let weeks = [];
            let week = [];

            for (let day of this.days) {
                week.push(day);
                if (week.length === 7) {
                    weeks.push(week);
                    week = [];
                }
            }

            return weeks;
        }
    },
    components: {
        CalendarDay,
        CurrentMonth,
        EventForm
    }
};
</script>
