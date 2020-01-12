<template>
	<div id="app">
		<app-header>Vhost-manager</app-header>
		<div class="container">
			<div class="list">
				<app-item
				v-for="(host, index) in hosts" :key="index"
				:host="host"
				:id="index"
				@app:delete="deleteItem"
			/>
			</div>
		</div>
	</div>
</template>

<script>
import appHeader from './components/Header.vue';
import appItem from './components/HostItem.vue'

const PORT = "3000";
const URL = `http://localhost:${PORT}`;

export default {
  name: 'app',
  components: {
	appHeader,
	appItem
  },
  data() {
	return {
		status: 'stopped',
		hosts: [],
		deleteMsg: ''
	}
  },
  watch: {
	deleteMsg: {
		handler(obj) {
			if(obj.error == '') {
				this.showData(obj.data);
			} else {
				this.showError(obj.error);
			}
		},
		deep: true
	} 
  },
  methods: {
	async getStatus() {
		const res = await fetch(`${URL}/status`)
		const json = await res.json();
		this.status = json.status;
	},
	async getHosts() {
		await this.getStatus();
		if(this.status == 'active') {
			const res = await fetch(`${URL}/list`);
			const json = await res.json();
			this.hosts = json;
		} else {
			throw new Error('Serves is disabled');
		}
	},
	async deleteItem(url) {
		const delObj = {name: url}
		this.hosts = this.hosts.filter( el => el.url != url );
		const res = await fetch(`${URL}/delete`, {method: 'POST', body: JSON.stringify(delObj)});
		const json = await res.json();
		this.deleteMsg = json;
	},
	showData(value) {
		alert(value.normalize()) //TODO: Вместо Alert сделать красивый вывод!
	},
	showError(value) {
		value;
	}
  },
	mounted() {
		this.getHosts();
  }
}
</script>

<style>
		.list {
			margin-top: 4s0px;
		}
		.title-wrapper {
			display: flex;
			justify-content: center;
		}
		.title {
			font-size: 3em;	
		}
</style>
