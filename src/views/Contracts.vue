<template>
	<div class="window white">
	        <fieldset class='contractsdialog'>
	            <legend>Contracts</legend>
	            <fieldset>
	            	<legend>Curve Pool Registry</legend>
	                	<a href = "https://etherscan.io/address/0x7002B727Ef8F5571Cb5F9D70D13DBEEb4dFAe9d1">
	                		<img class='icon' :src="publicPath + 'curveIcons/curve-registry.svg'"> <span class='text'>Curve Registry address</span>
	                	</a>
	                	<a href = "https://etherscan.io/address/0xc1DB00a8E5Ef7bfa476395cdbcc98235477cDE4E">
	                		<img class='icon' :src="publicPath + 'curveIcons/curve-registry.svg'"> <span class='text'>Curve calc address</span>
	                	</a>
	                	<a href = "https://github.com/curvefi/curve-pool-registry/blob/b17/doc/notebook/playbook.ipynb">
	                		<img class='icon' :src="publicPath + 'curveIcons/curve-registry.svg'"> 
	                		<span class='text'>Curve Registry docs</span>
	                	</a>
	            </fieldset>
	            <fieldset>
	            	<legend>Insurance</legend>
	                	<a href = "https://app.nexusmutual.io/#/SmartContractCover">
	                		<img class='icon' :src="publicPath + 'curveIcons/nexusmutual.png'"> <span class='text'>Stablecoin pools - curvev2.nexusmutual.eth</span>
	                	</a>
	                	<a href = "https://etherscan.io/address/0xc1DB00a8E5Ef7bfa476395cdbcc98235477cDE4E">
	                		<img class='icon' :src="publicPath + 'curveIcons/nexusmutual.png'"> <span class='text'>BTC pools - curvebtc.nexusmutual.eth</span>
	                	</a>
	            </fieldset>
	            <fieldset v-for = '(addresses, i) in contractAddresses'>
	            	<legend>{{allPools[i]}}</legend>
	                	<a :href = "'https://etherscan.io/address/' + addresses.swap">
	                		<img class='icon' :src="getTokenUrl(i)"> <span class='text'>swap address</span>
	                	</a>
	                	<a :href = "'https://etherscan.io/address/' + addresses.token">
	                		<img class='icon' :src="getTokenUrl(i)"> 
	                		<span class='text'>[{{tokenNames[i].ticker}}] {{tokenNames[i].name}} token address</span>
	                	</a>
	                	<a :href = "'https://etherscan.io/address/' + depositZaps[i].deposit" v-show="!['ren', 'sbtc'].includes(allPools[i])">
	                		<img class='icon' :src="getTokenUrl(i)"> <span class='text'>deposit address</span>
	                	</a>
	                	<a :href= "'https://etherscan.io/address/' + rewardsAddresses[allPools[i]]" v-show="['susdv2', 'sbtc', 'y', 'iearn'].includes(allPools[i])">
	                		<img class='icon' :src="getTokenUrl(i)"> <span class='text'>staking rewards address</span>
	                	</a>
	                	<a href="https://etherscan.io/address/0x9fe350DfA5F66bC086243F21A8F0932514316627" v-show="['ren'].includes(allPools[i])">
	                		<img class='icon' :src="getTokenUrl(i)"> <span class='text'>old adapter address</span>
	                	</a>
	                	<a :href="'https://etherscan.io/address/' + adapterAddresses[allPools[i]]" v-show="['ren','sbtc'].includes(allPools[i])">
	                		<img class='icon' :src="getTokenUrl(i)"> <span class='text'>adapter address</span>
	                	</a>
	                	<a href='https://etherscan.io/token/0x0bc529c00c6401aef6d220be8c6ea1667f6ad93e' v-show="['y', 'iearn'].includes(allPools[i])">
	                		<img class='icon' :src="publicPath + 'curveIcons/yfi.png'">
	                		YFI token
	                	</a>
	            </fieldset>
	        </fieldset>
	    </div>
</template>

<script>
	import allabis from '../allabis'

	export default {
		data: () => ({
			allPools: ['compound', 'usdt', 'y', 'busd', 'susdv2', 'pax', 'ren', 'sbtc'],
			tokenNames: [
				{ name: 'cCurve', ticker: 'cCrv' },
				{ name: 'tCurve', ticker: 'tCrv' },
				{ name: 'yCurve', ticker: 'yCrv' },
				{ name: 'bCurve', ticker: 'bCrv' },
				{ name: 'sCurve', ticker: 'sCrv' },
				{ name: 'pCurve', ticker: 'pCrv' },
				// { name: 'tbtcCurve', ticker: 'tbtcCrv' },
				{ name: 'renCurve', ticker: 'renCrv' },
				{ name: 'sbtcCurve', ticker: 'sbtcCrv' },
			]
		}),
		computed: {
			contractAddresses() {
				return Object.keys(allabis).filter(pool => !['y', 'susd', 'tbtc'].includes(pool)).map(pool => 
					({swap: allabis[pool].swap_address, token: allabis[pool].token_address})
				)
			},
			depositZaps() {
				return Object.keys(allabis).filter(pool => !['y', 'susd', 'tbtc'].includes(pool)).map(pool => 
					({deposit: allabis[pool].deposit_address, token: allabis[pool].token_address})
				)
			},
			publicPath() {
				return process.env.BASE_URL
			},
			adapterAddresses() {
				return {
					ren: '0x73aB2Bd10aD10F7174a1AD5AFAe3ce3D991C5047',
					sbtc: '0xAEade605D01FE9a8e9C4B3AA0130A90d62167029',
				}
			},
			rewardsAddresses() {
				return {
					susdv2: '0xdcb6a51ea3ca5d3fd898fd6564757c7aaec3ca92',
					sbtc: '0x13C1542A468319688B89E323fe9A3Be3A90EBb27',
				}
			},
		},
		methods: {
			getTokenUrl(i) {
				let publicPath = process.env.BASE_URL
				return publicPath + 'curveIcons/' + this.tokenNames[i].ticker + '.png'
			},
		},
		metaInfo: {
	      title: 'Curve.fi :: Contracts',
	      meta: [
	        {'property': 'og:title', 'content': 'beta.curve.fi/contracts'},
	        {'property': 'og:url', 'content': 'https://curve.fi/contracts'},
	        {'property': 'og:type', 'content': 'website'},
	        {'property': 'og:description', 'content': 'Curve is an exchange liquidity pool on Ethereum designed for extremely efficient stablecoin trading'},
	        {'property': 'og:image', 'content': '/curve.png'},
	        {'name': 'twitter:card', 'content': 'summary_large_image'},
	        {'name': 'twitter:title', 'content': 'beta.curve.fi/contracts'},
	        {'name': 'twitter:site', 'content': '@CurveFinance'},
	        {'name': 'twitter:creator', 'content': '@CurveFinance'},
	        {'name': 'twitter:description', 'content': 'Curve is an exchange liquidity pool on Ethereum designed for extremely efficient stablecoin trading'},
	        {'name': 'twitter:url', 'content': 'https://curve.fi/contracts'},
	        {'name': 'twitter:image', 'content': '/curve.png'},
	      ]
	    },
	}
</script>

<style scoped>
	legend {
		text-align: center;
	}
	.contractsdialog a {
		display: flex;
		padding: 0.1em;
	}
	.contractsdialog a img {
		margin-right: 10px;
	}
	.contractsdialog div {
		display: block;
		margin-top: 10px;
	}
	.contractsdialog a:hover {
	    background-color: blue;
    	color: white;
	}
</style>