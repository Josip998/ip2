<template>
	<div class="home">
		<v-main>
			<v-container>
				<v-dialog v-model="dialog" width="500">
					<template v-slot:activator="{ on, attrs }">
						<div style="margin-bottom: 20px">
							<v-btn color="success" v-bind="attrs" v-on="on"
								>+ Dodaj novo</v-btn
							>
						</div>
					</template>

					<v-card>
						<v-card-title class="text-h5 grey lighten-2">
							Dodavanje nove lige
						</v-card-title>
						<v-card-body>
							<v-container>
								<v-text-field label="Gender"></v-text-field>
								<v-text-field label="Area name"></v-text-field>
								<v-text-field label="Format"></v-text-field>
							</v-container>
						</v-card-body>
					</v-card>
				</v-dialog>

				<v-text-field
					v-model="keyword"
					label="Search"
					outlined
					clearable
				></v-text-field>

				<div
					style="display: flex; justify-content: flex-end; align-items: center"
				>
					<v-autocomplete
						v-model="filter"
						:items="items.map((item) => item.Format)"
						label="Odaberite ligu"
					></v-autocomplete>
					<v-btn color="red" small @click="filter = null">Obrisi</v-btn>
				</div>

				<div class="no-results" v-if="filteredItems.length === 0">
					Nema rezultata
				</div>

				<template v-if="items.length > 0">
					<v-row>
						<v-col
							v-for="item in filteredItems"
							:key="item.CompetitionId"
							sm="12"
							lg="3"
							md="4"
						>
							<v-card height="200">
								<b>{{ item.Name }}</b>
								<div>
									Gender:
									<span
										:style="{ color: item.Gender === 'Male' ? 'blue' : 'red' }"
									>
										{{ item.Gender }}</span
									>
								</div>
								<div style="color: gray">
									{{ item.AreaName }}
								</div>

								<v-btn
									@click="
										showSeason = true;
										dataModal = item.Seasons;
									"
									small
									color="green"
								>
									Vidi sezone
								</v-btn>

								<div class="card-footer">
									{{ item.Format }}
								</div>

								<v-dialog v-model="dialog" width="500">
									<template v-slot:activator="{ on, attrs }">
										<div style="margin-top: 10px">
											<v-btn small color="primary" sm v-bind="attrs" v-on="on"
												>Uredi</v-btn
											>
										</div>
									</template>

									<v-card>
										<v-card-title class="text-h5 grey lighten-2">
											Dodavanje nove lige
										</v-card-title>
										<v-card-body>
											<v-container>
												<v-text-field :value="item.Gender" label="Gender"
													>{
												</v-text-field>
												<v-text-field
													label="Area name"
													:value="item.AreaName"
												></v-text-field>
												<v-text-field
													:value="item.Format"
													label="Format"
												></v-text-field>
											</v-container>
										</v-card-body>
									</v-card>
								</v-dialog>
							</v-card>
						</v-col>
					</v-row>
					<v-row>
						<v-col>
							<v-pagination
								v-model="page"
								:length="length / limit"
							></v-pagination>
						</v-col>
					</v-row>
				</template>

				<div v-else>Loading....</div>

				<Modal :dataModal="dataModal" v-if="showSeason" @close="onCloseModal" />
			</v-container>
		</v-main>
	</div>
</template>

<script>
	import Modal from "@/components/Modal.vue";
	export default {
		name: "HomeView",
		data: () => ({
			url: "https://api.sportsdata.io/v3/soccer/scores/json/Competitions?key=",
			items: [],
			filteredItems: [],
			key: "7de0e399390e4353b8eaf14dadd1c0a9&fbclid=IwAR3nUKUWLZ-MgaSbvdUpcbGqGAomur9w_28knrYgMFYcnwsd_HcvML5xktA",
			keyword: "",
			filter: "",
			page: 1,

			limit: 8,
			showSeason: false,
			dataModal: {},
			length: 0,
		}),
		methods: {
			setFilters() {
				let res = [...this.items];
				if (this.filter) {
					res = this.items.filter((item) => item.Format === this.filter);
				}
				if (this.keyword) {
					res = res.filter((item) =>
						item.Name.toLowerCase()
							.trim()
							.includes(this.keyword.toLowerCase().trim())
					);
				}

				this.length = res.length;
				this.filteredItems = res.slice(
					(this.page - 1) * this.limit,
					this.page * this.limit
				);
			},
			getData() {
				this.axios.get(this.url + this.key).then((res) => {
					this.items = res.data;
					this.setFilters();
				});
			},
			onCloseModal() {
				this.showSeason = false;
			},
		},
		created() {
			this.getData();
		},
		watch: {
			filter: function () {
				this.setFilters();
			},
			keyword: function () {
				this.setFilters();
			},
			page: function () {
				this.setFilters();
			},
		},
		components: { Modal },
	};
</script>
<style lang="scss" scoped>
	.card-footer {
		position: absolute;
		width: 100%;
		top: 80%;
		text-align: center;
		background: rgba(97, 97, 97, 0.207);
		height: 20%;
	}

	.no-results {
		color: gray;
	}

	.show-season {
	}
</style>

