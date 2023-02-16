<template>
	<div :class="{'app': true, 'win': isWin, 'loss': isLoss}">
		<span>Player {{ name }} (Slot {{ mySlotIndex }})</span>
		<div class="phase">
			<span v-if="phase == 'planning'">Place your ships</span>
			<span v-if="phase == 'waiting'">Wait for the other player</span>
			<span v-if="phase == 'shoot'">Shoot your enemy</span>
			<span v-if="phase == 'win'">You won!</span>
			<span v-if="phase == 'defeat'">You loose!</span>
		</div>
		<!-- This is an active board to click - firstly for setting up your ships, next to shoot your opponent -->
		<single-map 
			v-if="displayBoard"
			class="map"
			v-on:elementClicked="elementClicked"
			:blockPicking="blockPicking"
			ref="currentBoard"
		></single-map>
		<ship-picker 
			ref="shipPicker"
			v-on:allShipsPicked="allShipsPicked($event)"
			v-on:shipPicked="shipPicked"
		></ship-picker>

		<hr class="horizontal">

		<!-- the following board is just an overview of your board after the game is started -->
		<single-map
			v-if="gameIsOn"
			class="map small-map"
			:blockPicking="true"
			:overrideCoordinates="myPickedNodes"
			ref="referralBoard"
		></single-map>
	</div>
</template>

<script>
import SingleMap from './components/SingleMap.vue';
import ShipPickerVue from './components/ShipPicker.vue';
import { io } from 'socket.io-client';

import Swal from 'sweetalert2';

const WAITING = 'waiting';

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
			phase: 'planning',
		};
	},
	mounted: function () {
		var names = [
			"Krzysztof", 
			"Anna", 
			"Wojciech", 
			"Maciej", 
			"Radosław", 
			"Karol", 
			"Zuzanna", 
			"Marcin", 
			"Małgorzata",
			"Katarzyna",
		];
		this.name = names[Math.floor(Math.random() * 10)];

		this.socket = io("localhost:8082");

		this.socket.on('yourId', id => this.mySlotIndex = id);

		this.socket.on('opponentReady', function () {
			Swal.fire({
				toast: true,
				position: 'top-end',
				title: 'Opponent ready!',
			});
		});

		this.socket.on('blockPicking', () => {
			this.blockPicking = true;
			this.phase = WAITING;
		});

		this.socket.on('yourTurn', () => {
			this.blockPicking = false;
			this.phase = 'shoot';
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

			if (data.hitBy === this.mySlotIndex)
				this.$refs.shipPicker.killShip(data.ship.id);

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

			this.$refs.shipPicker.resetShips();

			this.$nextTick(() => {
				this.displayBoard = true;
			});
		});

		this.socket.on('gameEnd', () => {
			this.blockPicking = true;
		});

		this.socket.on('youWin', () => {
			this.isWin = true;
			this.phase = 'win';
		});

		this.socket.on('youLoose', () => {
			this.isLoss = true;
			this.phase = 'defeat';
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
			this.phase = WAITING;

			data.forEach(ship => {
				this.myPickedNodes.push(...ship.nodes);
			});

			this.socket.emit('allShipsPicked', data);
		},
		shipPicked: function (shipData) {
			shipData.nodes.forEach(node => {
				this.$refs.currentBoard.killElement(node);
			})
		},
	},
}

</script>

<style>
html, body {
	overflow: hidden;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 1rem;
}
/* 
.map-row, .map-column {
  display: inline-block;
} */

.map {
  width: 100%;
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

.phase {
	font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
	margin-top: 1rem;
	margin-bottom: 1rem;
}

</style>
