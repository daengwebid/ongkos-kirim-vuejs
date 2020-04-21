<template>
	<div class="container" id="app">
		<div class="row mt-3">
			<div class="col-md-8">
				<div class="card">
					<div class="card-body">
						<div class="form-group">
							<label for="">Nama Lengkap</label>
							<input type="text" class="form-control">
						</div>
						<div class="form-group">
							<label for="">No Telp</label>
							<input type="text" class="form-control">
						</div>
						<div class="form-group">
							<label for="">Alamat</label>
							<input type="text" class="form-control">
						</div>
						<div class="form-group">
							<label for="">Propinsi</label>
							<select class="form-control" v-model="province_id" @change="getCities">
								<option value="">Pilih</option>
								<option v-for="(row, index) in provinces" :key="'provinces'+index" :value="row.id">{{ row.name }}</option>
							</select>
						</div>
						<div class="form-group">
							<label for="">Kota/Kabupaten</label>
							<select class="form-control" v-model="city_id" @change="getDistricts">
								<option value="">Pilih</option>
								<option v-for="(row, index) in cities" :key="'cities'+index" :value="row.id">{{ row.name }}</option>
							</select>
						</div>
						<div class="form-group">
							<label for="">Kecamatan</label>
							<select class="form-control" v-model="district_id" @change="getCouriers">
								<option value="">Pilih</option>
								<option v-for="(row, index) in districts" :key="'districts'+index" :value="row.id">{{ row.name }}</option>
							</select>
						</div>
						<div class="form-group">
							<label for="">Kurir</label>
							<select class="form-control" v-model="courier">
								<option value="">Pilih</option>
								<option v-for="(row, index) in couriers" :key="'couriers'+index" :value="row">{{ row.courier }} - {{ row.service }} (Rp {{ row.cost }})</option>
							</select>
						</div>
					</div>
				</div>
			</div>
			<div class="col-md-4">
				<div class="card">
					<div class="card-header">
						<h4 class="card-title">Summary</h4>
					</div>
					<div class="card-body">
						<app-summary :courier="courier" />
					</div>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
import axios from 'axios'
import Summary from './components/Summary.vue'

export default {
	name: 'App',
	created() {
		this.getProvinces()
	},
	data() {
		return {
			api_key: 'MASUKKAN API KEY RUANGAPI',
			provinces: [],
			province_id: '',
			cities: [],
			city_id: '',
			districts: [],
			district_id: '',
			couriers: [],
			courier: ''
		}
	},
	methods: {
		getProvinces() {
			axios.get('https://ruangapi.com/api/v1/provinces', {
				headers: {
					Authorization: this.api_key
				}
			}).then((response) => {
				let provinces = response.data.data.results
				this.provinces = provinces
			})
		},
		getCities() {
			axios.get('https://ruangapi.com/api/v1/cities', {
				params: {
					province: this.province_id
				},
				headers: {
					Authorization: this.api_key
				}
			}).then((response) => {
				let cities = response.data.data.results
				this.cities = cities
			})
		},
		getDistricts() {
			axios.get('https://ruangapi.com/api/v1/districts', {
				params: {
					city: this.city_id
				},
				headers: {
					Authorization: this.api_key
				}
			}).then((response) => {
				let districts = response.data.data.results
				this.districts = districts
			})
		},
		getCouriers() {
			axios.post('https://ruangapi.com/api/v1/shipping', {
				origin: 22,
				destination: this.district_id,
				weight: 700,
				courier: 'jne,jnt'
			}, {
				headers: {
					Authorization: this.api_key
				}
			}).then((response) => {
				let couriers = response.data.data.results
				this.couriers = couriers
			})
		}
	},
	components: {
		'app-summary': Summary
	}
}
</script>