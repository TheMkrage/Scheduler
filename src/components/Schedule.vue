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
			<tr>
				<td>{{dynoSize}}</td> 
				<td>{{frequency}}</td>
				<td>{{lastRun}}</td>
				<td>{{ today }} {{nextRunTime}} UTC</td>
			</tr>
		</table>
	</div>
</template>

<script>
export default {
	name: 'Schedule',
	data () {
		return {
			msg: 'Welcome to Your Vue.js App'
		}
	}, 
	props: {
		dynoSize: Number,
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
	}
}
</script>

<style type="text/css"></style>
