<template>
	<div :class="{'app': true, 'win': isWin, 'loss': isLoss}">
		<!-- This is an active board to click - firstly for setting up your ships, next to shoot your opponent -->
		<single-map 
			v-if="displayBoard"
			class="map"
			v-on:elementClicked="elementClicked"
			:blockPicking="blockPicking"
			ref="currentBoard"
		></single-map>
		<!-- the following board is just an overview of your board after the game is started -->
		<single-map
			v-if="gameIsOn"
			class="map small-map"
			:blockPicking="true"
			:overrideCoordinates="myPickedNodes"
			ref="referralBoard"
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
			isWin: false,
			isLoss: false,
			mySlotIndex: undefined,
		};
	},
	mounted: function () {
		this.socket = io("localhost:8082");

		this.socket.on('yourId', id => this.mySlotIndex = id);

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

		this.socket.on('yourTurn', () => {
			this.blockPicking = false;
		});

		this.socket.on('hit', (data) => {
			var coordinates = data.coordinates;
			Swal.fire({
				toast: true,
				title: 'Hit!',
				showConfirmButton: false,
				position: 'top-right',
				timer: 1000,
			})

			if (data.hitBy === this.mySlotIndex)
				this.$refs.currentBoard.hitElement(coordinates);
			else if (data.hitBy !== undefined)
				this.$refs.referralBoard.hitElement(coordinates);
		});

		this.socket.on('miss', (data) => {
			var coordinates = data.coordinates;
			Swal.fire({
				toast: true,
				title: 'Miss!',
				showConfirmButton: false,
				position: 'top-right',
				timer: 1000,
			});

			if (data.missBy === this.mySlotIndex)
				this.$refs.currentBoard.missElement(coordinates);
			else if (data.missBy !== undefined)
				this.$refs.referralBoard.missElement(coordinates);
		});

		this.socket.on('hitAndDead', (data) => {
			Swal.fire({
				title: 'DEAD SHIP',
				text: 'Player ' + data.hitBy + ' killed opponent\'s ship ID: ' + data.ship.id,
				showConfirmButton: false,
				timer: 1500,
			});

			data.ship.nodes.forEach(node => {
				if (data.hitBy === this.mySlotIndex)
					this.$refs.currentBoard.killElement(node);
				else if (data.hitBy !== undefined)
					this.$refs.referralBoard.killElement(node);
			});
		});

		this.socket.on('gameIsOn', () => {
			this.gameIsOn = true;

			this.displayBoard = false;

			this.$nextTick(() => {
				this.displayBoard = true;
			});
		});

		this.socket.on('gameEnd', () => {
			this.blockPicking = true;
		});

		this.socket.on('youWin', () => {
			this.isWin = true;
		});

		this.socket.on('youLoose', () => {
			this.isLoss = true;
		});

		// this.socket.on('connect', () => {
		// 	// Swal.fire({
		// 	// 	toast: true,
		// 	// 	text: 'Connected to server',
		// 	// })
		// })

		this.socket.on('disconnect', function () {
			console.log('Disconnected');
		});

	},
	methods: {
		elementClicked: function (element) {
			if (this.gameIsOn) {
				this.socket.emit('shoot', {
					coordinates: element,
				});
			} else {
				this.socket.emit('clicked', element);
				this.$refs.shipPicker.elementClicked(element);
			}
		},
		allShipsPicked: function (data) {
			data.forEach(ship => {
				this.myPickedNodes.push(...ship.nodes);
			});

			this.socket.emit('allShipsPicked', data);
		},
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

.win {
	background-color: green;
}

.loss {
	background-color: red;
}

</style>
