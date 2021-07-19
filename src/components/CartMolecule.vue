<template>
	<div class="container">
		<div class="cart d-flex align-items-end">
			<div class="me-2">
				<i class="bi bi-cart4 fs-2"></i>
				<span className="fs-3 fw-bold text-success">
					<sup>{{ cartTotalItems }}</sup>
				</span>
			</div>
			<h4>{{ cartName }}</h4>
		</div>
		<hr />
		<table class="table">
			<thead>
				<tr>
					<th scope="col">{{ tableProductName }}</th>
					<th scope="col">{{ tableQuantityName }}</th>
					<th class="text-end" scope="col">{{ tablePriceName }}</th>
				</tr>
			</thead>
		</table>
		<div v-if="cartItems.length === 0">
			<p>{{ cartEmptyMessage }}</p>
		</div>
		<div v-else v-for="item in cartItems" :key="item.id">
			<CartAtom :cartItem="item" v-bind="$attrs" />
		</div>
		<div v-if="cartItems.length !== 0">
			<p class="fw-bold">
				{{ cartTotalPriceName + " " + updateCartTotalPrice() }} €
			</p>
		</div>
		<div>
			<button
				type="button"
				class="btn btn-dark"
				@click="checkoutMessage"
				:disabled="!hasItems"
			>
				{{ cartButtonName }}
			</button>
		</div>
	</div>
</template>

<script>
import CartAtom from "./CartAtom.vue";

export default {
	name: "CartMolecule",
	components: {
		CartAtom,
	},
	props: {
		cartItems: {
			type: Array,
		},
	},
	data() {
		return {
			tableProductName: "Producto",
			tableQuantityName: "Cantidad",
			tablePriceName: "Importe",
			cartName: "Carrito",
			cartEmptyMessage: "Añade elementos al carrito",
			cartButtonName: "Realizar Pedido",
			cartTotalPriceName: "Precio Total:  ",
			cartTotalPrice: 0,
		};
	},
	methods: {
		updateCartTotalPrice() {
			this.cartTotalPrice = this.cartItems.reduce(
				(a, c) => a + c.price * c.qty,
				0
			);
			return this.cartTotalPrice.toFixed(2);
		},
		checkoutMessage() {
			alert("Pedido realizado");
		},
	},
	computed: {
		hasItems() {
			if (this.cartItems.length != 0) return true;
			else return false;
		},
		cartTotalItems() {
			return this.cartItems.reduce((a, c) => a + c.qty, 0);
		},
	},
};
</script>

<style scoped>
</style>