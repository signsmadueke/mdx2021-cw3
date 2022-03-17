<template>
	<div class="shopping-cart">
		<div class="cart">
			<h1>Shopping Cart</h1>
			<div class="items" v-if="cartItems.length > 0">
				<div
					class="item"
					v-for="(cartItem, index) in cartItems"
					:key="cartItem.lessonID"
				>
					<div class="details">
						<div class="image">
							<img
								v-bind:src="`https://signsmadueke.github.io/mdx2021-cw2-f/${cartItem.lesson.image}`"
								alt=""
							/>
						</div>
						<div class="text">
							<p>Subject: {{ cartItem.lesson.topic }}</p>
							<p>Location: {{ cartItem.lesson.location }}</p>
							<p>Price: {{ cartItem.lesson.price }}</p>
							<p>Space: {{ cartItem.space }}</p>
						</div>
					</div>
					<button v-on:click="removeLesson(index)" type="button">
						Remove
					</button>
				</div>
			</div>

			<div class="checkout" id="check-out-form">
				<h1>Checkout</h1>
				<div class="form">
					<input
						v-on:keyup="validateRegexCheckOut"
						type="text"
						placeholder="Name"
						v-model="checkOutName"
					/>
					<input
						v-on:keyup="validateRegexCheckOut"
						type="text"
						placeholder="Phone"
						v-model="checkOutNumber"
					/>
					<button
						v-bind:disabled="cannotCheckOut"
						v-on:click="checkOut(checkOutName, checkOutNumber)"
						type="button"
						class="checkout-btn"
					>
						Checkout
					</button>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	name: "Checkout",
	props: ["cartItems"],
	data() {
		return {
			checkOutName: "",
			checkOutNumber: "",
			cannotCheckOut: true,
		};
	},
	methods: {
		removeLesson: function (index) {
			this.$emit("removeLesson", index);
		},
		validateRegexCheckOut: function () {
			var nameRegexPattern = /^[a-zA-Z ]*$/;
			var numberRegexPattern = /^\d+$/;
			if (
				this.checkOutName.match(nameRegexPattern) &&
				this.checkOutNumber.match(numberRegexPattern)
			) {
				this.cannotCheckOut = false;
			} else {
				this.cannotCheckOut = true;
			}
		},
		checkOut: function (checkOutName, checkOutNumber) {
			this.$emit("checkOut", checkOutName, checkOutNumber);
		},
	},
};
</script>