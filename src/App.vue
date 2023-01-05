<template>
	<div class="app">
		<single-map 
			v-if="displayBoard"
			class="map"
			v-on:elementClicked="elementClicked"
			:blockPicking="blockPicking"
		></single-map>
		<single-map
			v-if="gameIsOn"
			class="map small-map"
			:blockPicking="true"
			:overrideCoordinates="myPickedNodes"
		></single-map>
		<ship-picker 
			ref="shipPicker"
			v-on:allShipsPicked="allShipsPicked($event)"
		></ship-picker>
		<br/>
		<br/>
	</div>
</template>

<script>
import SingleMap from './components/SingleMap.vue';
import ShipPickerVue from './components/ShipPicker.vue';
import { io } from 'socket.io-client';

import Swal from 'sweetalert2';

export default {
	name: 'App',
	components: {
		SingleMap: SingleMap,
		ShipPicker: ShipPickerVue,
	},
	data: function () {
		return {
			name: 'Karol',
			socket: null,
			blockPicking: false,
			gameIsOn: false,
			myPickedNodes: [],
			displayBoard: true,
		};
	},
	mounted: function () {
		this.socket = io("localhost:8082");

		this.socket.on('playerReady', function () {
			console.log('EEE');
			Swal.fire({
				toast: true,
				position: 'top-end',
				title: 'Player ready!',
			})
		});

		this.socket.on('blockPicking', () => {
			this.blockPicking = true;
		});

		this.socket.on('gameIsOn', () => {
			this.gameIsOn = true;

			this.displayBoard = false;

			this.$nextTick(() => {
				this.displayBoard = true;
			});
		});

		this.socket.on('connection', () => {
			console.log('Connected to socket');

			this.socket.emit('hello', 'world');    
		})

		this.socket.on('disconnect', function () {
			console.log('Disconnected');
		});

	},
	methods: {
		elementClicked: function (element) {
			this.socket.emit('clicked', element);
			this.$refs.shipPicker.elementClicked(element);
		},
		allShipsPicked: function (data) {
			data.forEach(ship => {
				this.myPickedNodes.push(...ship.nodes);
			});

			this.socket.emit('allShipsPicked', data);
		}
	},
}

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
/* 
.map-row, .map-column {
  display: inline-block;
} */

.map {
  width: 500px;
}

.small-map {
    width: 50%;
    margin: 0 auto;
}

/* .app {
  width: 100%;
  height: 100%;
} */

</style>
