<template>
	<header>
		<nav class="navbar navbar-dark bg-dark">
			<div class="container text-white">
				{{ title }}
				<a class="navbar-brand" href="#Admin">
					<i class="bi bi-shield-lock"></i>
				</a>
			</div>
		</nav>
		<div class="container">
			<div class="d-inline-flex mt-3 mb-4">
				<select
					class="form-select"
					v-model="selectedCategory"
					@change="onChange"
				>
					<option value="" selected>Todos los productos</option>
					<option :value="options[0].value">
						{{ options[0].label }}
					</option>
					<option :value="options[1].value">
						{{ options[1].label }}
					</option>
				</select>
			</div>
		</div>
	</header>
</template>

<script>
import CartAtom from "./CartAtom.vue";

export default {
	name: "Header",
	emits: ["category-selected"],
	components: {
		CartAtom,
	},
	data() {
		return {
			title: "TRABAJO FINAL | COMPRANDO CON VUE",
			options: [
				{
					label: "Palas",
					value: "palas",
				},
				{
					label: "Pelotas",
					value: "botes",
				},
			],
			selectedCategory: "",
		};
	},
	methods: {
		onChange() {
			this.$emit("category-selected", this.selectedCategory);
		},
	},
};
</script>

<style scoped>
</style>