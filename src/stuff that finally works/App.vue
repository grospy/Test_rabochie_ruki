<template>
  <div id="app">
		<div class="calendar-controls">
			<div v-if="message" class="notification is-success">{{ message }}</div>
				<div class="field">	<label class="label">Начальное число</label>
		    <div class="control">	<input v-model="newItemStartDate" class="input" type="date" /> </div> </div>
        <div class="field"> <label class="label">Конечное число</label>
				<div class="control"> <input v-model="newItemEndDate" class="input" type="date" />	</div>	</div>
				<button class="button is-info" @click="NewButtonEvent">Зафиксировать</button>
			</div> 	</div> 
</template> 
<script>
// Load CSS from the published version
import "../node_modules/extra-extensions/dist/style.css"
import "../node_modules/extra-extensions/static/css/default.css"
import "../node_modules/extra-extensions/static/css/holidays-us.css"
import { CalendarView, CalendarViewHeader, CalendarMath } from "extra-extensions" // published version
export default {
	name: "App",
	components: {
		CalendarView,
		CalendarViewHeader,
	},
	data() {
		return {
			/* Show the current month, and give it some fake items to show */
			showDate: this.thisMonth(1),
			message: "",
			items: [
				{
					id: "e11",
					startDate: this.thisMonth(29),
					title: "Same day 7",
				},
			],
		}
	},
	computed: {
		userLocale() {
			return CalendarMath.getDefaultBrowserLocale
		},
		dayNames() {
			return CalendarMath.getFormattedWeekdayNames(this.userLocale, "long", 0)
		},
	},
	mounted() {
		this.newItemStartDate = CalendarMath.isoYearMonthDay(CalendarMath.today())
		this.newItemEndDate = CalendarMath.isoYearMonthDay(CalendarMath.today())
	},

	methods: {
		thisMonth(d, h, m) {
			const t = new Date()
			return new Date(t.getFullYear(), t.getMonth(), d, h || 0, m || 0)
		},
		NewButtonEvent() {
			this.items.push({
				startDate: this.newItemStartDate,
				endDate: this.newItemEndDate,
				title: this.newItemTitle,
				id: "e" + Math.random().toString(36).substr(2, 10),
			})
			this.message = "This is the new start date: " + this.newItemStartDate + "This is the new end date:" + this.newItemEndDate
		},
	},
}
</script>
<style>
html,
body {
	height: 100%;
	margin: 0;
	background-color: #f7fcff;
}

#app {
	display: flex;
	flex-direction: row;
	font-family: Calibri, sans-serif;
	width: 95vw;
	min-width: 30rem;
	max-width: 100rem;
	min-height: 40rem;
	margin-left: auto;
	margin-right: auto;
}

.calendar-controls {
	margin-right: 1rem;
	min-width: 14rem;
	max-width: 14rem;
}

/* For long calendars, ensure each week gets sufficient height. The body of the calendar will scroll if needed */
.cv-wrapper.period-month.periodCount-2 .cv-week,
.cv-wrapper.period-month.periodCount-3 .cv-week,
.cv-wrapper.period-year .cv-week {
	min-height: 6rem;
}
</style>