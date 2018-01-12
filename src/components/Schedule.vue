<template>
	<div class="schedule">
		<input class="schedule-name" type="text" v-model="nameFieldText" :disabled="!isEditing"></input>
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
					<select v-model="dynoSizeSelection">
						<option>Free</option>
						<option>1</option>
						<option>2</option>
					</select>
				</td>
				<td>
					<select v-model="frequencySelection">
						<option>Daily</option>
						<option>Hourly</option>
						<option>Every 10 Minutes</option>
					</select>
				</td>
				<td>{{lastRun}}</td>
				<td>{{ today }} 
					<select v-model="nextRunTimeSelection">
						<option v-for="n in 48">{{ Math.round(n / 2) }}:{{ n % 2 == 0 ? "00" : "30" }}</option>
					</select> UTC
				</td>
			</tr>
		</table>
		<div class="schedule-buttons">
			<button class="main-button" :class="{ dominant: isEditing }" @click="toggleEditing">{{ isEditing ? "Save" : "Edit" }}</button>
			<button class="text-only-button" @click="destructiveButtonPressed">{{ isEditing ? "Cancel" : "Remove" }}</button>
		</div>
	</div>
</template>

<script>
export default {
	name: "Schedule",
	data () {
		return {
			isEditing: this.isEditingInitial,
			nameFieldText: this.name,
			dynoSizeSelection: this.dynoSize,
			frequencySelection: this.frequency,
			nextRunTimeSelection: this.nextRunTime,
			hasCompletedInitialSave: false
		}
	}, 
	props: {
		name: String,
		dynoSize: String,
		frequency: String,
		lastRun: String,
		nextRunTime: String,
		isEditingInitial: Boolean,
		id: Number // id of the schedule
	}, 
	computed: {
		today() {
			var today = new Date();
			var tomorrow = new Date();
			tomorrow.setDate(today.getDate() + 1);

			var monthNames = ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"];

			var month = monthNames[tomorrow.getMonth()];
			var day = tomorrow.getDay();
			return month + " " + day;
		}
	}, 
	methods: {
		toggleEditing() {
			if (this.isEditing) { 
				this.hasCompletedInitialSave  = true;
				var scheduleToEmit = { id:this.id, name:this.nameFieldText, dynoSize:this.dynoSizeSelection, frequency:this.frequencySelection, nextRunTime:this.nextRunTimeSelection, lastRun: this.lastRun };
				this.$emit("save", scheduleToEmit);
			}
			this.isEditing = !this.isEditing;
		},

		destructiveButtonPressed() {
			// if the schedule is being edited and has already saved once then cancel, else remove the selection from the list
			if (this.isEditing && this.hasCompletedInitialSave) {
				this.isEditing = false;
			} else {
				var scheduleToEmit = { id:this.id, name:this.nameFieldText, dynoSize:this.dynoSizeSelection, frequency:this.frequencySelection, nextRunTime:this.nextRunTimeSelection, lastRun: this.lastRun };
				this.$emit("remove", scheduleToEmit);
			}
		}
	}
}
</script>

<style type="text/css">

button {
	outline: none;
	font-size: 12px;
}

button:hover {
	cursor: pointer;
}

table {
	padding-bottom: 10px;
	padding-top: 10px;
	border-top: 1px solid #d3d3d3;
}

td, th {
	padding-left: 20px;
	text-align: left;
	font-size: 12px;
	color: black;
}

th {
	color: #7D7D8E;
	text-transform: uppercase;
}

.text-only-button {
	border: 0px solid gray;
	color: #79589F;
	background-color: transparent;
}

.text-only-button:hover {
	text-decoration: underline;
}

.main-button {
	background-color: white;
	color: #79589F;
	border-radius: 4px;
	height: 24px;
	padding-left: 12px;
	padding-right: 12px;
	font-weight: 700;
	border: 1px solid #79589F;
}

.main-button:hover {
	background-color: #e6e6e6;
}

.main-button.dominant {
	background-color: #79589F;
	color: white;
}

.main-button.dominant:hover {
	background-color: #654887;
}

.schedule {
	width: 50%;
	border: 1px solid gray;
	border-radius: 6px;
	padding-top: 15px;
	margin-bottom: 25px; 
	margin-left: auto;
	margin-right: auto;
	box-sizing: border-box;
}
.schedule-buttons {
	border-radius: 0px 0px 4px 4px;
	background-color: #F5F5F7;
	border: 1px solid #DCDCDD;
	padding: 15px;
	text-align: left;
}
   
.schedule-name {
	color: #424242;
	font-size: 14px;
	margin-bottom: 15px;
	margin-top: 10px;
	height: 25px;
	width: 90%;
	padding-left: 10px;
	border: 1px solid gray;
	border-radius: 4px;
}
</style>

