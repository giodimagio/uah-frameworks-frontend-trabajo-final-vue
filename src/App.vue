<template>
	<Header @category-selected="updateSelectedCategory" />
	<div class="container">
		<div class="row">
			<div class="col-9">
				<div class="row">
					<div
						class="col-4"
						v-for="productData in filteredProductsByCategory"
						:key="productData.id"
					>
						<ProductAtom
							@add-to-cart="addToCart"
							:product="productData"
							v-bind="$attrs"
						/>
					</div>
				</div>
			</div>
			<div class="col-3">
				<div class="row">
					<CartMolecule
						:cartItems="cart"
						@increase-item="addToCart"
						@decrease-item="removeFromCart"
					/>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
import ProductAtom from "./components/ProductAtom.vue";
import CartAtom from "./components/CartAtom.vue";
import CartMolecule from "./components/CartMolecule.vue";
import Header from "./components/Header.vue";
import db from "../db.js";

export default {
	name: "App",
	components: {
		CartAtom,
		CartMolecule,
		ProductAtom,
		Header,
	},
	data() {
		return {
			cart: [],
			data: [],
			selectedCategory: "",
		};
	},
	created() {
		this.data = db.items;
		console.log("created");
	},
	updated() {
		console.log("updated");
	},
	methods: {
		addToCart(product) {
			let indexCart = this.cart.findIndex((x) => x.id === product.id);
			let indexData = this.data.findIndex((x) => x.id === product.id);

			if (indexCart != -1) {
				this.cart[indexCart].qty++;
			} else {
				product.qty = 1;
				this.cart.push(product);
			}
			this.data[indexData].stock--;
		},
		removeFromCart(product) {
			let indexCart = this.cart.findIndex((x) => x.id === product.id);
			let indexData = this.data.findIndex((x) => x.id === product.id);

			if (this.cart[indexCart].qty > 1) {
				this.cart[indexCart].qty--;
			} else if (this.cart[indexCart].qty == 1) {
				this.cart.splice(indexCart, 1);
			}
			this.data[indexData].stock++;
		},
		updateSelectedCategory(selectedCategory) {
			this.selectedCategory = selectedCategory;
		},
	},
	computed: {
		filteredProductsByCategory: function () {
			if (this.selectedCategory === "") {
				return this.data;
			} else {
				return this.data.filter(
					(product) =>
						!product.category.indexOf(this.selectedCategory)
				);
			}
		},
	},
};
</script>

<style>
#app {
	font-family: Avenir, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	background-image: linear-gradient(to top, #dfe9f3 0%, white 100%);
	min-height: 720px;
}
</style>
