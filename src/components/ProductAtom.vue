<template>
	<div class="card cardSize">
		<div class="card-body">
			<img
				:src="product.imageUrl"
				class="card-img-top box-img"
				:alt="product.id"
			/>

			<h5 class="card-title fw-bold">
				{{ product.name }}
			</h5>

			<div class="card-subtitle mb-2">
				<h6 class="text-success" v-if="product.stock >= 5">
					{{
						productAvailableName +
						" " +
						product.stock +
						" " +
						product.category
					}}
				</h6>
				<h6
					class="text-warning"
					v-else-if="product.stock >= 2 && product.stock < 5"
				>
					{{
						productFewAvailableName +
						" " +
						product.stock +
						" " +
						product.category
					}}
				</h6>
				<h6 class="text-warning" v-else-if="product.stock == 1">
					{{
						productLastAvailableName +
						" " +
						product.stock +
						" " +
						product.category.slice(0, -1)
					}}
				</h6>
				<h6 class="text-danger" v-else>
					{{ productNotAvailableName }}
				</h6>
			</div>

			<div class="fs-6 fw-light">
				<small>
					<pre>{{ product.description }}</pre>
				</small>
			</div>
			<p class="fw-bold">{{ product.price }} €</p>
			<button
				type="button"
				class="btn btn-primary"
				@click="addProductToCart"
				:disabled="!inStock"
			>
				{{ buttonAddToCartName }}
			</button>
		</div>
	</div>
</template>

<script>
export default {
	name: "ProductAtom",
	props: {
		product: {
			type: Object,
		},
	},
	data() {
		return {
			productAvailableName: "Disponibles",
			productFewAvailableName: "Quedan solo",
			productLastAvailableName: "Queda solo",
			productNotAvailableName: "Agotado",
			buttonAddToCartName: "Agregar a la cesta",
		};
	},
	methods: {
		addProductToCart() {
			this.$emit("add-to-cart", this.product);
		},
	},
	computed: {
		inStock() {
			if (this.product.stock > 0) return true;
			else return false;
		},
	},
};
</script>

<style scoped>
img {
	border-radius: 10px;
}
.box-img:hover {
	transform: scale(1.5);
	box-shadow: 5px 5px 5px black;
	transition: 1s;
}
.cardSize {
	width: 300px;
}
</style>
