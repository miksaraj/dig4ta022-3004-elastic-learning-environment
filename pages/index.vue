<template>
	<div>
		<b-card
			title="Psykologian 1.kurssi"
			img-src="/img/aloitus.jpg"
			img-alt="Aloitus"
			img-right
			style="width: 100%;"
			class="hero-card"
		>
			<b-card-text>
				Tervetuloa lukion psykologian 1. kurssille. Tällä kurssilla on
				seitsemän eri lukua, jotka johdattelevat sinua ymmärtämään
				ihmismielen toimintaa. Klikkaa ensimmäistä korttia "Aloitus".
			</b-card-text>
		</b-card>
		<options-row :type="type" />
		<card-deck v-if="view === 'cards'" :items="items" :type="type" />
		<accordion-list v-if="view === 'list'" :items="items" />
	</div>
</template>

<style scoped>
/* muokka näitä ja tämän cardin propseja, jos haluat erilaisen/paremman tästä */
.hero-card {
	margin-bottom: 2rem;
	background-color: var(--color-secondary);
	color: #ffffff;
}
</style>

<script>
import CardDeck from '~/components/CardDeck.vue'
import OptionsRow from '~/components/OptionsRow.vue'
import AccordionList from '~/components/AccordionList.vue'
export default {
	components: {
		CardDeck,
		OptionsRow,
		AccordionList
	},
	data() {
		return {
			type: 'chapters'
		}
	},
	computed: {
		items() {
			return this.$store.state.chapters.list
		},
		view() {
			return this.$store.state.settings.chaptersView
		}
	}
}
</script>
