<template>
	<div class="container">
		<div>
			<img
				src="../static/oppia.io_logo_oranssi.png"
				alt="Oppia.io"
				width="260"
				height="90"
			/>
			<form-group :items="items" />
			<div class="submit">
				<a class="button" @click="signup">
					Rekisteröidy
				</a>
			</div>
		</div>
	</div>
</template>

<style>
.submit .button {
	display: inline-block;
	border-radius: 4px;
	border: 1px solid #ffffff;
	color: #ffffff;
	text-decoration: none;
	padding: 10px 30px;
}

.submit .button:hover {
	color: var(--color-main);
	background-color: #ffffff;
}

.submit {
	padding-top: 15px;
}
</style>

<script>
import FormGroup from '~/components/FormGroup.vue'
export default {
	layout: 'login',
	components: {
		FormGroup
	},
	data() {
		return {
			items: [
				{
					id: 'email',
					label: 'Sähköpostiosoite',
					input: '',
					type: 'email',
					required: true
				},
				{
					id: 'name',
					label: 'Etu- ja sukunimi',
					input: '',
					type: 'text'
				},
				{
					id: 'username',
					label: 'Käyttäjätunnus',
					input: '',
					type: 'text',
					required: true
				},
				{
					id: 'pwd',
					label: 'Salasana',
					input: '',
					type: 'password',
					required: true
				},
				{
					id: 'pwd-2',
					label: 'Salasana uudestaan',
					input: '',
					type: 'password',
					required: true
				}
			]
		}
	},
	methods: {
		signup() {
			const userDetails = {}
			let pwd = ''
			let pwd2 = ''
			for (let i = 0; i < this.items.length; i++) {
				if (this.items[i].id === 'pwd') {
					pwd = this.items[i].input
					if (pwd.length < 8) {
						alert('Salasanan tulee olla vähintään 8 merkkiä pitkä.')
						return
					}
				} else if (this.items[i].id === 'pwd-2') {
					pwd2 = this.items[i].input
					if (pwd2.length < 8) {
						alert('Salasanan tulee olla vähintään 8 merkkiä pitkä.')
						return
					}
				} else {
					// check if input in required fields
					if (
						this.items[i].input === '' &&
						this.items[i].id !== 'name'
					) {
						alert(this.items[i].label + ' puuttuu.')
						return
					}
					userDetails[this.items[i].id] = this.items[i].input
				}
			}
			if (pwd !== pwd2) {
				alert('Salasanat eivät täsmää keskenään!')
				return
			} else {
				// base64 encode password (not secure enough for real production)
				userDetails.pwd = btoa(pwd)
			}
			this.$store.dispatch('auth/login')
			this.$store.dispatch('profile/update', userDetails)
			this.$router.push('/')
		}
	}
}
</script>
