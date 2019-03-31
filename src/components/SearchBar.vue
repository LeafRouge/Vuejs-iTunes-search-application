<template>
	<v-app>
		<v-content>
			<h1 class="title">Search application using iTunes's Search API</h1>
			<v-card>
				<v-card-text class="searchBarCard">
					<v-form @submit.prevent="submitRequest()">
						<v-layout row>
							<v-text-field
								class="mt-0"
								color="white"
								dark clearable
								v-model="searchString"
								label="Enter Artist Name"
							>
							</v-text-field>
							<v-btn color=white type="submit">
								<v-icon>search</v-icon>
							</v-btn>
						</v-layout>
					</v-form>
				</v-card-text>
				<v-card-text v-if="items.length">
					<v-layout class="items-wrapper">
						<div
							class="individual-cards"
							v-for="(item, index) in items"
							:key="index"
						>
							<v-avatar
								:tile="true"
								size="200"
								class="image-container transition-image"
								@click="viewDetails(item)"
								>
								<div class="container">
									<img :src="getImageUrl(item)"
										alt="Cover"
										class="image-items transition-image"
									>
									<div class="middle">
										<div class="text-details">Details</div>
									</div>
								</div>
							</v-avatar>
							<v-flex>
								<div class="white-text">{{ item.collectionName }}</div>
								<div class="grey-text inline-text">
									<p v-if="item.kind">( </p>{{ item.kind | capitalizeMedia }}<p v-if="item.kind"> )</p>
								</div>
								<div class="grey-text">{{ item.artistName}}</div>
								<div class="grey-text">{{ getReleaseDate(item) }}</div>
							</v-flex>
						</div>
					</v-layout>
				</v-card-text>
			</v-card>
		</v-content>
	</v-app>
</template>

<script>
	import axios from 'axios';

	export default {
		data () {
			return {
				searchString: '',
				items: []
			}
		},
		watch: {
			search: function (param) {
				if (!param) {
					this.items = []
				}
			}
		},
		methods: {
			getReleaseDate (item) {
				return item.releaseDate.substr(0, 4)
			},
			submitRequest () {
				let result = this
				return axios.get(`https://itunes.apple.com/search?term=${this.searchString}`)
				.then((response) => {
					result.items = response.data.results
				})
			},
			getImageUrl (item) {
				return item.artworkUrl100.replace("100x100", "200x200");
			},
			viewDetails (item) {
				window.open(item.collectionViewUrl);
			}
		},
		filters: {
			capitalizeMedia (value) {
				if (!value)
					return '';
				value = value.toString();
				return value.charAt(0).toUpperCase() + value.slice(1);
			}
		}
	}
</script>

<style scoped>
	.title {
		font-family: 'Arimo', sans-serif;
		color: white;
		padding: 30px;
		text-align: center;
	}

	.v-card {
		width: 80%;
		text-align: center;
		margin: 0 auto;
	}

	.searchBarCard {
		padding-bottom: 0!important;
	}

	.container {
		position: relative;
		padding: 0 !important;
	}
	
	.transition-image {
		opacity: 1;
		display: block;
		width: 100%;
		height: auto;
		transition: .5s ease;
		backface-visibility: hidden;
	}

	.middle {
		opacity: 0;
		position: absolute;
		top: 50%;
		left: 50%;
		-webkit-transform: translate(-50%, -50%);
		-ms-transform: translate(-50%, -50%);
		text-align: center;
		cursor: pointer;
	}

	.items-wrapper {
		flex-wrap: wrap;
		justify-content: center;
	}

	.container:hover .transition-image {
		opacity: 0.3;
	}

	.container:hover .middle {
		opacity: 1;
	}

	.text-details {
		background-color: #2cb5e8;
		color: white;
		font-size: 16px;
		padding: 16px 32px;
	}

	.image-container {
		position: relative;
		width: 100%;
		height: auto;
	}

	.individual-cards {
		width: 200px;
		text-align: center !important;
		margin-left: 2% !important;
		margin-right: 2% !important;
		margin-top: 1% !important;
		margin-bottom: 1% !important;
	}

	@keyframes pop{
		50%  { transform: scale(1.02); }
	}

	.grey-text {
		color: #bdbdbd !important;
	}

	.bold-text {
		font-weight: 500 !important;
	}

	.white-text {
		font-weight: 500 !important;
		color: white !important;
		font-size: 17px !important;
		font-weight: 400 !important;
	}

	.inline-text {
		display: -webkit-inline-box;
		white-space: nowrap;
		overflow: hidden;
		text-overflow: ellipsis;
		text-align: center !important;
		margin-bottom: -20px;
	}

	div {
		white-space: nowrap;
		overflow: hidden;
		text-overflow: ellipsis;
	}

	.v-text-field {
		margin-top: 0!important;
	}
	
	.v-card__title,
	.v-card__text {
		background: #004d66;
	}
</style>