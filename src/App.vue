<template>
	<div class="container">
		<div class="movie-header">
			<img src="@/assets/logo.png" alt="VueLogo" />
			<h3>Movies Search App</h3>
			<form @submit.prevent="onSubmit">
				<input type="text" v-model="searchValue" placeholder="Movie name" />
				<button type="submit" class="btn movie-header__button">Find</button>
			</form>
		</div>
		<p v-if="movies == [] || movies == null">404 - Movies Not found</p>
		<ul v-else class="movie-list">
			<li v-for="(movie, index) in movies" :key="index" class="movie">
				<img :src="movie.Poster" alt="" />
				<h4>{{ movie.Title }}, {{ movie.Year }}</h4>
			</li>
		</ul>
	</div>
</template>

<script>
import axios from "axios";
export default {
	name: "App",
	data: () => ({
		movies: [],
		searchValue: "place",
	}),
	methods: {
		onSubmit() {
			this.getMovies();
		},
		getMovies() {
			axios
				.get(`https://www.omdbapi.com/?s=${this.searchValue}&apikey=62f7dfee`)
				.then((response) => (this.movies = response.data.Search))
				.catch((error) => console.log(error));
		},
	},
	mounted() {
		this.getMovies();
	},
};
</script>

<style lang="scss">
// simple reset
* {
	padding: 0;
	margin: 0;
	box-sizing: border-box;
}
// base styles
#app {
	font-family: Avenir, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	color: #2c3e50;
	margin-top: 60px;
}
.container {
	max-width: 1070px;
	width: 100%;
	padding-right: 15px;
	padding-left: 15px;
	margin: 0 auto;
}
h1,
h2,
h3,
h4,
h5,
.title {
	margin-bottom: 12px;
	font-weight: bold;
}
ul {
	list-style: none;
}
input {
	padding: 11px 18px;
	font-size: 18px;
	border: 2px solid #323232;
}
.btn,
button {
	padding: 16px 35px;
	background: #323232;
	color: #ffffff;
	font-weight: bold;
	cursor: pointer;
	&:hover{
		background: #444444;
	}
}
// movies styles
.movie-header {
	margin-bottom: 40px;
	img {
		width: 80px;
	}
	&__button {
		margin-left: 12px;
		border: none;
	}
	form {
		display: flex;
		justify-content: center;
		align-items: center;
	}
}
.movie-list {
	display: grid;
	grid-template-columns: repeat(5, 1fr);
	@media (screen and max-width:960px){
		grid-template-columns: repeat(3, 1fr);
	}
	@media (screen and max-width:480px){
		grid-template-columns: repeat(2, 1fr);
	}
gap: 20px 40px;
	justify-content: center;
	align-content: center;
}
.movie {
	img {
		max-height: 400px;
	}
}
</style>
