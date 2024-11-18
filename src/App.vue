<script>
	import axios from 'axios'

	export default {
		data() {
			return {
				city: "",
				cityPrevious: "",
				error: "",
				info: null,
			}
		},

		computed: {
			cityName() {
				return `«${this.city}»`
			},
			temperature() {
				return "Temperature is " + this.info.data.main.temp + "C"
			},
			feelsLike() {
				return "Feels like: " + this.info.data.main.feels_like + "C"
			},
			minTemp() {
				return "Minimum temperature: " + this.info.data.main.temp_min + "C"
			},
			maxTemp() {
				return  "Maximum temperature: " + this.info.data.main.temp_max + "C"
			},
			windSpeed() {
				return "Wind speed: " + this.info.data.wind.speed + " m/s"
			},
		},

		methods: {
			getWeather() {
				console.log("Method 'this.getWeather' called")
				if (this.city.trim().length < 2) {
					this.error = "The name of city is less than one character";
					return false;
				}
				this.cityPrevious = this.city;

				this.error = "";

				axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=873529a1b10fe28c7bafafeb2fc55dcf`).then(
					res => (this.info = res)
					)
			}
		}
	}
// 873529a1b10fe28c7bafafeb2fc55dcf
</script>

<template>
	<div class="wrapper">
		<h1>Vite Weather App</h1>
		<p>Find out weather {{ city == "" ? "in your city" : `in ${cityName}` }}</p>

		<div class="input_block">
			<!-- @input="this.city = $event.target.value" -->
			<input type="text" v-model="city" placeholder="Enter city name">
			<p class="error">{{ error }}</p>
			<button v-show="city != ''" @click="getWeather()">Get Weather</button>
			<div class="answer_block">
				<div v-if="info != null && city == cityPrevious" class="block weather">
					<p class="main_temp">{{ temperature }}</p>
					<p class="feels_like">{{ feelsLike }}</p>
					<p class="min_temp">{{ minTemp }}</p>
					<p class="max_temp">{{ maxTemp }}</p>
					<p class="wind">{{ windSpeed }}</p>
				</div>
			</div>
		</div>


	</div>
</template>

<style scoped>
	.wrapper {
		width: 50vw;
		min-height: 60vh;
		background-color: #18181a;
		border-radius: 50px;
		padding: 3%;

		text-align: center;
		color: #fff;
	}

	.wrapper h1 {
		margin-top: 5%;
	}
	.wrapper p {
		margin-top: 2%;
	}

	.input_block {
		margin-top: 10%;
		display: flex;
		flex-flow: column nowrap;
		align-items: center;
	}

	.input_block input {
		width: 70%;
	}
	.input_block button {
		padding: 3% 7%;
		margin-top: inherit;
	}

	.block.weather {
		margin-top: 20px;
		width: 100%;
	}
	.block.weather p{
		text-align: left;
		width: 100%;
		margin-bottom: 5%;
	}
</style>
