<template>
	<div class="row align-items-center">
		<div class="col-4">
			<img :src="cartItem.imageUrl" :alt="cartItem.id" />
		</div>
		<div class="col-4">
			<div class="d-flex justify-content-between align-items-center">
				<button
					@click="decreaseNumberOfItems"
					className="btn btn-danger btn-circle fw-bold"
				>
					{{ minusSign }}
				</button>
				<div>
					<span>{{ cartItem.qty }}</span>
				</div>
				<button
					@click="increaseNumberOfItems"
					:disabled="!inStock"
					className="btn btn-success btn-circle fw-bold"
				>
					{{ plusSign }}
				</button>
			</div>
		</div>
		<div class="col-4">
			<div>
				<span class="float-end"
					>{{ (cartItem.price * cartItem.qty).toFixed(2) }} €
				</span>
			</div>
		</div>
	</div>
	<p>{{ cartItem.name }}</p>
	<hr />
</template>

<script>
export default {
	name: "CartAtom",
	emits: ["increase-item", "decrease-item"],
	props: {
		cartItem: {
			type: Object,
		},
	},
	data () {
		return {
			plusSign: "+",
			minusSign: "-"
		}
	},
	methods: {
		increaseNumberOfItems() {
			this.$emit("increase-item", this.cartItem);
		},
		decreaseNumberOfItems() {
			this.$emit("decrease-item", this.cartItem);
		},
	},
	computed: {
		inStock() {
			if (this.cartItem.stock > 0) return true;
			else return false;
		},
	},
};
</script>

<style scoped>
img {
	height: 50px;
}
.btn-circle {
	width: 30px;
	height: 30px;
	text-align: center;
	padding: 0px;
	font-size: 20px;
	border-radius: 15px;
}
</style>
