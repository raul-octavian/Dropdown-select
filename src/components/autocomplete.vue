<template>
	<div class="flex-center container-flex">
		<form
			id="address-form"
			class="container-flex flex-center"
			autocomplete="off"
			action=""
		>
			<div class="container">
				<fieldset>
					<legend>Input the country you are from:</legend>
					<div class="countryInput">
						<label for="country" class="flex-self-start">Country:</label>
						<input
							id="country"
							type="text"
							name="country-Input"
							v-model="countryInput"
							placeholder="street, nr, Zip Code, city"
							@input="startFiler()"
						/>

						<transition
							name="ballmove"
							enter-active-class="growY"
							leave-active-class="shrinkY"
						>
							<select
								v-show="countryFilter"
								name="countryList"
								:size="setSelectSize"
                                v-model="countryValue"
                                @keydown.prevent.enter="setCountryValueEnter()"
							>
								<option
									v-for="(country, index) in countryFilter"
									:key="index"
									value:country.name
                                    title: country.name
                                    name:country.name
									@click="setCountryValue(country.name)"
									>{{ country.name }}
								</option>
							</select>
						</transition>
					</div>
				</fieldset>
			</div>
		</form>
	</div>
</template>

<script>
	import axios from "axios";
	export default {
		data() {
			return {
				countryInput: "",
				countries: null,
				countryFilter: null,
				countryValue: ""

				// selectSize: this.setSelectSize
			};
		},
		computed: {
			setSelectSize() {
				var size = "1";
				if (this.countryFilter) {
					if (this.countryFilter.length > 10) {
						size = "10";
					} else if (this.countryFilter.length > 1) {
						size = this.countryFilter.length.toString();
					}
				}

				return size;
			}
		},
		methods: {
			findAdress() {
				this.countryFilter = this.countries.filter(country =>
					country.name.toLowerCase().includes(this.countryInput.toLowerCase())
				);
			},

			startFiler() {
				if (this.countryInput.length > 0) {
					this.findAdress();
				} else {
					this.countryFilter = null;
				}
			},
			setCountryValue(item) {
				this.countryInput = item;
				this.$emit("displayCountryName", this.countryInput);

				this.countryFilter = null;
			},

			setCountryValueEnter() {
				this.countryInput = this.countryValue;
				this.$emit("displayCountryName", this.countryInput);
				this.countryFilter = null;
			}
		},
		mounted() {
			const baseURI = "https://restcountries.eu/rest/v2/all";
			axios
				.get(baseURI)
				.then(result => {
					this.countries = result.data;
				})
				.catch(error => {
					console.log(error.message);
				});
		}
	};
</script>

<style lang="scss">
	#address-form {
		background-color: var(--secondary-color);
		color: var(--text-color);
		min-height: 400px;
		min-width: 400px;
		max-width: 300px;
		padding: 3rem 0;
		border: 3px solid var(--accent-color);
		// display: flex;
		// justify-content: center;
		// flex-direction: row;

		.container {
			width: 80%;
		}
		.countryInput {
			min-width: 90%;
			display: flex;
			justify-content: flex-start;
			flex-direction: column;
			align-items: center;
			overflow-x: hidden;
		}
		fieldset {
			width: 90%;
			padding: 1rem;
		}
		legend {
			width: 80%;
		}

		input {
			width: 80%;
			font-size: 0.9rem;
			padding: 0.5rem 1rem;
			margin: 0.5rem 0 1rem 0;
			border: solid 1px var(--main-color);

			&::placeholder {
				font-size: 0.7rem;
				font-style: italic;
			}
		}
		select {
			width: 80%;
			font-size: 0.9rem;
			padding: 0.2rem 0.5rem;
			margin: 0 0 1rem 0;
			border: solid 1px var(--secondary-color);
			overflow-x: hidden;
			&::-webkit-scrollbar {
				display: none;
			}
			option {
				width: 100%;
				overflow-x: hidden;
			}
		}
	}
</style>