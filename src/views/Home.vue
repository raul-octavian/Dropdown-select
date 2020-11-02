<template>
	<div class="home">
		<autocomplete> </autocomplete>
	</div>
</template>

<script>
	// @ is an alias to /src
	import autocomplete from "../components/autocomplete";

	export default {
		name: "Home",
		components: {
			autocomplete: autocomplete
		}
	};
</script>
