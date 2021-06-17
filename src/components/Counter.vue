<template>
	<div>
		<form @submit.prevent="showRemaining()">
			<select v-model="day">
				<label>Jour</label>
				<option v-for='index in 31' :key='index'>{{index}}</option>
			</select>
			<select v-model="month">
				<option v-for='index in 12' :key='index'>{{index}}</option>
			</select>
			<button type="submit">Soumettre</button>
		</form>
		{{displayDays}} jour<span v-if="displayDays > 1">s</span> 
		{{displayHours}} :
		{{displayMinutes}} : 
		{{displaySeconds}}
	</div>
</template>
<script>
	export default {
		data(){
			return{
				displayDays: 0,
				displayHours: 0,
				displayMinutes: 0,
				displaySeconds: 0,
				day: 1,
				month: 1
			}
		},
		computed: {
			_seconds() {
				return 1000
			},
			_minutes(){
				return this._seconds * 60
			},
			_hours(){
				return this._minutes * 60
			},
			_days(){
				return this._hours * 24
			}
		},
		methods: {
			showRemaining(){
				const timer = setInterval(() => {
					const now = new Date()
					const year = (this.month < now.getMonth() + 1) || (this.month == now.getMonth() + 1 && this.day < now.getDate()) ? 2022 : 2021 
					const end = new Date(year, this.month - 1, this.day, 0, 0, 0, 0)
					const distance = end.getTime() - now.getTime()
					if (distance<0){
						clearInterval(timer)
						this.displayDays = this.displayHours = this.displayMinutes = this.displaySeconds = 0
						return
					}
					const days = Math.floor(distance / this._days)
					const hours = Math.floor((distance % this._days)/this._hours)
					const minutes = Math.floor((distance % this._hours)/this._minutes)
					const seconds = Math.floor((distance % this._minutes) / this._seconds)
					this.displayMinutes = minutes < 10 ? "0" + minutes : minutes;
					this.displaySeconds = seconds < 10 ? "0" + seconds : seconds;
					this.displayHours = hours < 10 ? "0" + hours : hours;
					this.displayDays = days < 10 ? "0" + days : days;
				}, 1000)
			}
		}
	}
</script>
<style lang="scss" scoped>
</style>