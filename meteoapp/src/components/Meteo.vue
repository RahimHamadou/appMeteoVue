<template>
	<div class="container">
		<h1 class="mb-4">App Meteo Vue.js</h1>

		<div class="form-groupe mb-5">
			<label for="position">Entrez le nom d'une ville</label>
			<input type="text" id="position" class="form-control" v-model="requete" v-on:keypress="goMeteo" />
			<!-- le v-on:keypress="goMeteo" peut etre remplacer par @keypress.enter= "goMeteo" , ce qui va permettre d'enlever le if dans methods et tot ce qui est lié a l'argument e -->

			<!-- <h3>{{ requete }}</h3>
			<h3>{{ cible }}</h3> -->
		</div>

		<div class="w-75 m-auto" v-if="temps">
			<h3 class="text-center mb-3">Position : {{ temps.name }}</h3>

			<div class="card text-center p-5">
				<!-- la methode toFixed permet de formater les chiffres -->
				<p class="texte-affichage">Temperature : {{ temps.main.temp.toFixed() }}</p>

				<p class="texte-affichage">Temps: {{ temps.weather[0].description }}</p>
			</div>
		</div>
	</div>
</template>

<script>
import axios from "axios";

export default {
	name: "Meteo",
	data() {
		return {
			requete: "",
			cible: "",
			temps: undefined,
			api_code: "a93612679cb91948ff40c2d5ea5a893b",
			url_recherche: "https://api.openweathermap.org/data/2.5/weather?",
		};
	},
	methods: {
		goMeteo(e) {
			if (e.key == "Enter") {
				console.log(this.requete);
				axios
					.get(`${this.url_recherche}q=${this.requete}&units=metric&APPID=${this.api_code}&lang=fr`)
					.then((reponse) => {
						this.temps = reponse.data;
					});
				this.requete = "";
			}
		},
	},
};
</script>

<style>
h1,
label,
h3 {
	color: #f1f1f1;
	text-transform: capitalize;
	font-weight: 700;
}
</style>
