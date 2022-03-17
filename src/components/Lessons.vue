<template>
	<div>
		<h1>All Lessons</h1>
		<br />
		<div class="sort">
			<select v-model="attribute">
				<option
					v-for="(sortAttribute, key) in sortAttributes"
					:key="sortAttribute"
					v-bind:value="sortAttribute"
				>
					{{ key }}
				</option>
			</select>

			<select v-model="order">
				<option
					v-for="(sortOrder, key) in sortOrders"
					:key="sortOrder"
					v-bind:value="sortOrder"
				>
					{{ key }}
				</option>
			</select>
		</div>

		<div class="lessons" v-if="lessons.length > 0">
			<div
				v-for="lesson in sortedLessons"
				:key="lesson.id"
				class="lesson"
			>
				<div class="details">
					<div class="image">
						<img
							v-bind:src="`https://signsmadueke.github.io/mdx2021-cw2-f/${lesson.image}`"
							alt=""
						/>
					</div>
					<div class="text">
						<p>Subject: {{ lesson.topic }}</p>
						<p>Location: {{ lesson.location }}</p>
						<p>Price: ${{ lesson.price }}</p>
						<p>Spaces: {{ lesson.space }}</p>
					</div>
				</div>
				<button
					v-bind:disabled="lesson.space <= 0"
					v-on:click="addLesson(lesson._id, lesson)"
					type="button"
				>
					Add to Cart
				</button>
			</div>
		</div>
		<div v-else class="row grid-1">
			<p style="margin: auto">No result found</p>
		</div>
	</div>
</template>

<script>
export default {
	name: "Lessons",
	props: ["lessons"],
	methods: {
		addLesson: function (lessonID, lesson) {
			this.$emit("addLesson", lessonID, lesson);
		},
	},
	data() {
		return {
			order: "asc",
			attribute: "subject",
			sortOrders: {
				Ascending: "asc",
				Descending: "desc",
			},

			sortAttributes: {
				Subject: "subject",
				Location: "location",
				Price: "price",
				Spaces: "spaces",
			},
		};
	},
	computed: {
		sortedLessons() {
			function compareSubjectAsc(a, b) {
				if (a.topic > b.topic) return 1;
				if (a.topic < b.topic) return -1;
				return 0;
			}
			function compareSubjectDesc(a, b) {
				if (a.topic > b.topic) return -1;
				if (a.topic < b.topic) return 1;
				return 0;
			}

			function compareLocationAsc(a, b) {
				if (a.location > b.location) return 1;
				if (a.location < b.location) return -1;
				return 0;
			}
			function compareLocationDesc(a, b) {
				if (a.location > b.location) return -1;
				if (a.location < b.location) return 1;
				return 0;
			}

			function comparePriceAsc(a, b) {
				if (a.price > b.price) return 1;
				if (a.price < b.price) return -1;
				return 0;
			}
			function comparePriceDesc(a, b) {
				if (a.price > b.price) return -1;
				if (a.price < b.price) return 1;
				return 0;
			}

			function compareSpacesAsc(a, b) {
				if (a.space > b.space) return 1;
				if (a.space < b.space) return -1;
				return 0;
			}
			function compareSpacesDesc(a, b) {
				if (a.space > b.space) return -1;
				if (a.space < b.space) return 1;
				return 0;
			}

			if (this.order == "asc") {
				if (this.attribute == "subject") {
					// eslint-disable-next-line vue/no-side-effects-in-computed-properties
					return this.lessons.sort(compareSubjectAsc);
				} else if (this.attribute == "location") {
					// eslint-disable-next-line vue/no-side-effects-in-computed-properties
					return this.lessons.sort(compareLocationAsc);
				} else if (this.attribute == "price") {
					// eslint-disable-next-line vue/no-side-effects-in-computed-properties
					return this.lessons.sort(comparePriceAsc);
				} else {
					// eslint-disable-next-line vue/no-side-effects-in-computed-properties
					return this.lessons.sort(compareSpacesAsc);
				}
			} else {
				if (this.attribute == "subject") {
					// eslint-disable-next-line vue/no-side-effects-in-computed-properties
					return this.lessons.sort(compareSubjectDesc);
				} else if (this.attribute == "location") {
					// eslint-disable-next-line vue/no-side-effects-in-computed-properties
					return this.lessons.sort(compareLocationDesc);
				} else if (this.attribute == "price") {
					// eslint-disable-next-line vue/no-side-effects-in-computed-properties
					return this.lessons.sort(comparePriceDesc);
				} else {
					// eslint-disable-next-line vue/no-side-effects-in-computed-properties
					return this.lessons.sort(compareSpacesDesc);
				}
			}
		},
	},
};
</script>
