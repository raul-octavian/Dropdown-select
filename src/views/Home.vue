<template>
	<div class="home">
		<autocomplete @displayCountryName="displayCountryName"> </autocomplete>
		<div class="container-flex flex-center">
			<h2>Your country of choice is {{ country }}</h2>
		</div>
	</div>
</template>

<script>
	// @ is an alias to /src
	import autocomplete from "../components/autocomplete";

	export default {
		name: "Home",
		data() {
			return {
				country: ""
			};
		},
		components: {
			autocomplete: autocomplete
		},
		methods: {
			displayCountryName(e) {
				this.country = e;
			}
		}
	};
</script>
