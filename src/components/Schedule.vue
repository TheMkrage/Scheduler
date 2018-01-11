<template>
	<div class="schedule">
		<input id="schedule-name" type="text"></input>
		<table style="width:100%">
			<tr>
				<th>Dyno Size</th> 
				<th>Frequency</th>
				<th>Last Run</th>
				<th>Next Run</th>
			</tr>
			<tr v-if="!isEditing">
				<td>{{ dynoSize }}</td> 
				<td>{{ frequency }}</td>
				<td>{{ lastRun }}</td>
				<td>{{ today }} {{ nextRunTime }} UTC</td>
			</tr>
			<tr v-else>
				<td>
					<select v-model="dynoSize">
						<option>Free</option>
						<option>1</option>
						<option>2</option>
					</select>
				</td>
				<td>
					<select v-model="frequency">
						<option>Daily</option>
						<option>Hourly</option>
						<option>Every 10 Minutes</option>
					</select>
				</td>
				<td>{{lastRun}}</td>
				<td>{{ today }} 
					<select v-model="nextRunTime">
						<option v-for="n in 48">{{ Math.round(n / 2) }}:{{ n % 2 == 0 ? '00' : '30' }}</option>
					</select> UTC
				</td>
			</tr>
		</table>
		<button class="button" :class="{ dominant: !isEditing }" @click="toggleEditing">{{ isEditing ? 'Save' : 'Edit' }}</button>
	</div>
</template>

<script>
export default {
	name: 'Schedule',
	data () {
		return {
			isEditing: false,
		}
	}, 
	props: {
		dynoSize: String,
		frequency: String,
		lastRun: String,
		nextRunTime: String
	}, 
	computed: {
		today() {
			var today = new Date();
			var tomorrow = new Date();
			tomorrow.setDate(today.getDate() + 1);

			var monthNames = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'];

			var month = monthNames[tomorrow.getMonth()];
			var day = tomorrow.getDay();
			return month + ' ' + day;
		}
	}, 
	methods: {
		toggleEditing() {
			this.isEditing = !this.isEditing;
		}
	}
}
</script>

<style type="text/css">
	.button {
	background-color: white;
	color: black;
}
.button.dominant {
	background-color: purple;
	color: white;
}

.schedule {
	width: 60%;
	border: 1px solid gray;
	border-radius: 6px;
	padding: 15px;
	margin-left: auto;
	margin-right: auto;
	box-sizing: border-box;
}
.schedule-name {
	margin-bottom: 15px;
	margin-top: 10px;
	height: 25px;
	width: 90%;
	border: 1px solid gray;
	border-radius: 4px;
}
</style>
