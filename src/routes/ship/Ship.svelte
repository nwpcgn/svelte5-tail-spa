<script>
	import ShipArranger from '../../lib/ShipArranger.svelte'
	const ships = [
		{ name: 'battleship', size: 4, cnt: 1 },
		{ name: 'cruiser', size: 3, cnt: 2 },
		{ name: 'destroyer', size: 2, cnt: 3 },
		{ name: 'submarine', size: 1, cnt: 4 }
	]
</script>

<ShipArranger
	width={10}
	height={10}
	{ships}
	colorScheme={['#fff', '#0ea5e9', '#bae6fd']}
	playerId="player1" />

