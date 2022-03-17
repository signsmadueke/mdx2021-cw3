<template>
	<div id="app">
		<div class="header">
			<form action="#" class="search">
				<input
					class="form-control"
					type="search"
					placeholder="Search for a lesson"
					aria-label="Search"
					v-on:keyup="searchLessons"
					v-model="searchTxt"
				/>
			</form>

			<button
				class="cart-btn"
				v-bind:disabled="cartItems.length <= 0"
				v-on:click="updateShowProduct"
			>
				Cart&nbsp;&nbsp;&nbsp;
				<span class="fas fa-cart-plus"></span>
				{{ cartCount() }}
			</button>
		</div>

		<div v-if="showProduct" id="product">
			<Lessons
				:lessons="lessons"
				v-on:addLesson="addLessonToCart"
			></Lessons>
		</div>

		<section v-else id="check-out">
			<Checkout
				:cartItems="cartItems"
				v-on:removeLesson="removeLessonFromCart"
				v-on:checkOut="checkOut"
			></Checkout>
		</section>
	</div>
</template>

<script>
import Lessons from "./components/Lessons.vue";
import Checkout from "./components/Checkout.vue";

export default {
	name: "App",
	components: {
		Lessons,
		Checkout,
	},
	data() {
		return {
			lessons: [],

			cartItems: [],
			showProduct: true,
			searchTxt: "",
		};
	},

	methods: {
		addLessonToCart: function (lessonID, lesson) {
			const lessonIndex = this.lessons.findIndex(
				(lesson) => lesson._id === lessonID
			);
			if (lessonIndex != -1) {
				this.lessons[lessonIndex].space -= 1;
			}
			this.addToCart(lessonID, lesson);
		},

		updateShowProduct: function () {
			this.showProduct = !this.showProduct;
		},
		addToCart: function (lessonID, lesson) {
			const itemIndex = this.cartItems.findIndex(
				(item) => item.lessonID === lessonID
			);
			if (itemIndex != -1) {
				this.cartItems[itemIndex].space += 1;
			} else {
				this.cartItems.push({
					lessonID: lessonID,
					space: 1,
					lesson: lesson,
				});
			}
		},
	},

	created: function () {
		fetch("https://mdx2021-cw2-b.herokuapp.com/collection/lessons/").then(
			(response) => {
				response.json().then((data) => {
					this.lessons = data;
				});
			}
		);
	},
};
</script>
