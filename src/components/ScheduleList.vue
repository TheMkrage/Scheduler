<template>
	<div>
		<Schedule v-for="schedule in schedules" v-bind="schedule" :key="schedule.id" @save="update" @remove="remove"></Schedule>
		<button @click="addAJob">Add a new Job</button>
	</div>
</template>

<script type="text/javascript">
import Schedule from './Schedule'

export default {
	name: 'ScheduleList',
	data () {
		return {
			schedules: [
					{ id: 4, name: "1j", dynoSize: "Free", frequency: "Daily", lastRun: "Yesterday", nextRunTime: "1:00" },
					{ id: 5, name: "2", dynoSize: "Free", frequency: "Daily", lastRun: "Yesterday", nextRunTime: "2:00" }
					// assume id is a primary key for schedules
				],
		}
	}, 
	components: {
		Schedule
	},
	methods: {
		update(scheduleToUpdate) {
			// get and update the matching schedule from schedules
			var matchingIndex = this.schedules.findIndex(x => x.id == scheduleToUpdate.id);
			this.$set(this.schedules, matchingIndex, scheduleToUpdate); 
			// must use vue's set to trigger reactivity
		},

		remove(scheduleToDelete) {
			console.log("bleh");
			// get and update the matching schedule from schedules
			var matchingIndex = this.schedules.findIndex(x => x.id == scheduleToDelete.id);
			this.$delete(this.schedules, matchingIndex); 
			// must use vue's set to trigger reactivity
		},

		addAJob() {
			var newSchedule = { id: Math.random(), name: "2", dynoSize: "Free", frequency: "Daily", lastRun: "Yesterday", nextRunTime: "3:00", isEditingInitial: true};
			this.schedules.push(newSchedule);
		}
	}
}
</script>

<style type="text/css">
	
</style>