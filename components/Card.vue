<template>
	<b-card :img-src="item.src" :img-alt="item.header">
		<h2>
			<nuxt-link v-if="type === 'chapters'" :to="'/chapters/' + item.id">
				{{ item.header }}
			</nuxt-link>
			<nuxt-link
				v-else
				:to="'/chapters/' + $route.params.id + '/' + item.id"
			>
				{{ item.header }}
			</nuxt-link>
		</h2>
		<p>{{ item.description }}</p>
		<card-list-group :items="subItems" :type="type" :id="item.id" />
		<template v-slot:footer>
			<progress-bar :item="item" />
		</template>
	</b-card>
</template>

<style scoped>
.card {
	height: 95%;
	margin-bottom: 1rem;
	width: 24vw;
	min-width: 20%;
}
@media screen and (max-width: 1124px) {
	.card {
		width: 36vw;
	}
}

@media screen and (max-width: 750px) {
	.card {
		width: 76vw;
	}
}
</style>

<script>
import { mapGetters } from 'vuex'
import CardListGroup from '~/components/CardListGroup.vue'
import ProgressBar from '~/components/ProgressBar.vue'
export default {
	name: 'Card',
	components: {
		CardListGroup,
		ProgressBar
	},
	data() {
		return {
			subItems: []
		}
	},
	computed: {
		...mapGetters({
			sectionsByChapter: 'sections/sectionsByChapter',
			sectionStructure: 'structure/sectionStructure',
			contentBySection: 'content/contentBySection'
		}),
		item() {
			return this.$attrs.item
		},
		type() {
			return this.$attrs.type
		}
	},
	mounted() {
		this.getSubItems()
	},
	methods: {
		/**
		 * Get the content under the title content depending on type,
		 * e.g. sections under chapters
		 */
		getSubItems() {
			if (this.type === 'chapters') {
				this.subItems = this.sectionsByChapter(this.item.id)
			} else {
				const content = this.sectionStructure(this.item.id)
				this.subItems = this.contentBySection(content)
			}
		}
	}
}
</script>
