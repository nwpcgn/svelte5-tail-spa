<script>
	import './ship.scss'
	import ShipArranger from './ShipArranger.js'
	let player1 = $state(),
		player2 = $state()

	const ships = [
		{ name: 'battleship', size: 4, cnt: 1 },
		{ name: 'cruiser', size: 3, cnt: 2 },
		{ name: 'destroyer', size: 2, cnt: 3 },
		{ name: 'submarine', size: 1, cnt: 4 }
	]

	let drawGameBoard = function () {
		let arranger1 = new ShipArranger(),
			arranger2 = new ShipArranger()

		try {
			arranger1.render(arranger1.arrange(ships), 'player1', [
				'blue',
				'red',
				'#fff'
			])
		} catch (ex) {
			player1.innerHTML = ex
		}

		try {
			arranger1.render(arranger2.arrange(ships), 'player2', [
				'blue',
				'green',
				'#fff'
			])
		} catch (ex) {
			player2.innerHTML = ex
		}
	}

	$effect(() => {
		drawGameBoard()
	})
</script>

<div class="gameboard">
	<div id="player1" bind:this={player1}></div>
	<div id="player2" bind:this={player2}></div>
	<input type="button" class="btn" on:click={drawGameBoard} value="Redraw" />
</div>
