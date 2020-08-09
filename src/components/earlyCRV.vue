<template>
	<div class='window white'>
		<fieldset>
			<legend>Historical LP <img class='icon small' :src="publicPath + 'logo.png'"> CRV Distribution</legend>
			<div class='simple-error'>
				CRV token <b>IS NOT</b> out yet. This page is to check your <b>approximate</b> historical LP share(5% of 3.03030303B)
			</div>
			<div class='input'>
				<label for='address'>Address:</label>
				<input id='address' type='text' v-model='address'>
			</div>
			<button @click='submit'>Check</button>

			<div class='earlyCRV' v-show='crv !== null'>
				Early &nbsp; <img class='icon small' :src="publicPath + 'logo.png'"> &nbsp; CRV amount: {{ crv }}
			</div>
			<div class='datainfo'>
				<a href='https://github.com/curvefi/early-user-distribution'>Calculation script</a>
				<a href='https://github.com/curvefi/CRV-distribution-data'>Data subgraphs</a>
				<a href='https://guides.curve.fi/everything-you-need-to-know-about-crv/'>Everything known about the CRV token article</a>
			</div>
			<div class='info-message gentle-message'>
				If you find inconsistencies, please contract us on 
				<a href="https://twitter.com/CurveFinance" rel='noopener noreferrer'>#Twitter</a>,
      			<a href="https://t.me/curvefi" rel='noopener noreferrer'>@Telegram</a>,
				<a href="https://discord.gg/9uEHakc" rel='noopener noreferrer'>@Discord</a>
			</div>
		</fieldset>
	</div>
</template>

<script>
    import { getters, contract as currentContract, gas as contractGas} from '../contract'

	export default {
		data: () => ({
			address: '',

			data: {},

			crv: null,
		}),

		async created() {
			this.$watch(()=>currentContract.default_account, (val, oldval) => {
                if(!val || !oldval) return;
                if(val.toLowerCase() != oldval.toLowerCase()) this.mounted();
            })
            this.$watch(()=>currentContract.initializedContracts, val => {
                if(val) this.mounted();
                console.timeEnd('initswap')
            })

		},

		mounted() {
			if(currentContract.initializedContracts) this.mounted();
		},

		computed: {
			publicPath() {
                return process.env.BASE_URL
            },
		},

		methods: {
			async mounted() {
				this.address = currentContract.default_account

				let data = await fetch('https://www.curve.fi/raw-stats/early-users.json')
				data = await data.json()
				this.data = data

				this.submit()
			},
			submit() {
				console.log(this.data)
				let crv = this.data[this.address.toLowerCase()] || 0
				this.crv = crv * 151515151
			},
		},
	}
</script>

<style scoped>
	legend {
		text-align: center;
	}
	.input {
		margin-top: 1em;
		display: flex;
		align-items: center;
	}
	.input input {
		margin-left: 1em;
		width: 31em;
	}
	button {
		margin-top: 1em;
	}
	.earlyCRV {
		display: flex;
		align-items: center;
		margin-top: 1em;
	}
	.earlyCRV img {
		margin-left: 0.5em;
		margin-right: 0.5em;
	}
	.info-message a {
		color: white;
	}
	.datainfo {
		margin-top: 1em;
	}
	.datainfo a {
		display: block;
		margin-top: 1em;
	}
</style>