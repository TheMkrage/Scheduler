<template>
	<div>
		<Schedule v-for="schedule in schedules" v-bind="schedule" :key="schedule.name"></Schedule>
	</div>
</template>

<script type="text/javascript">
import Schedule from './Schedule'
export default {
	name: 'ScheduleList',
	data () {
		return {
			schedules: [
				{ name: "1j", dynoSize: "Free", frequency: "Daily", lastRun: "Yesterday", nextRunTime: "1:00" },
				{ name: "2", dynoSize: "Free", frequency: "Daily", lastRun: "Yesterday", nextRunTime: "2:00" },
			]
		}
	}, 
	components: {
		Schedule
	}
}
</script>

<style type="text/css">
	
</style>