<template>
	<div>
		<Schedule v-for="schedule in schedules" v-bind="schedule" :key="schedule.id" @save="update" @remove="remove"></Schedule>
		<button class="addJob" @click="addAJob">Add a new Job</button>
	</div>
</template>

<script type="text/javascript">
import Schedule from './Schedule';
import axios from 'axios';

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
	created() {
		// load schedules from endpoint
		axios.get(`http://schedules.com/schedules`)
    .then(response => {
      this.schedules = response.data
    })
    .catch(e => {
      // respond to error if needed
    })
	},
	methods: {
		update(scheduleToUpdate) {
			// get and update the matching schedule from schedules
			var matchingIndex = this.schedules.findIndex(x => x.id == scheduleToUpdate.id);
			this.$set(this.schedules, matchingIndex, scheduleToUpdate); 
			// must use vue's set to trigger reactivity
			// update backend
			axios.put(`http://schedules.com/schedules`, scheduleToUpdate)
		},

		remove(scheduleToDelete) {
			// get and update the matching schedule from schedules
			var matchingIndex = this.schedules.findIndex(x => x.id == scheduleToDelete.id);
			this.$delete(this.schedules, matchingIndex); 
			// must use vue's delete to trigger reactivity
			// remove in backend
			axios.delete(`http://schedules.com/schedules`, { params: { id: scheduleToDelete.id } })
		},

		addAJob() {
			var newSchedule = { id: Math.random(), name: "", dynoSize: "Free", frequency: "Daily", lastRun: "Yesterday", nextRunTime: "3:00", isEditingInitial: true};
			this.schedules.push(newSchedule);
			// create in backend
			axios.post(`http://schedules.com/schedules`, newSchedule)
			// in reality, we would probably add the newSchedule to this.schedules in a handler from the post request because we would need the id object generated from the backend. For this example, I generated a random id and used it
		}
	}
}
</script>

<style type="text/css">
	button.addJob {
		width: 50%;
		padding-top: 12px;
		font-size: 18px;
		padding-bottom: 12px;
		background-color: #79589F;
		color: white;
		border-radius: 4px;
	}

	button.addJob:hover {
		background-color: #654887;
	}
</style>