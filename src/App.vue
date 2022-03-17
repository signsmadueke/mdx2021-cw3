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
export default {
	name: "App",
};
</script>
