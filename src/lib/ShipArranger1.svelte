<script>
	import { onMount } from 'svelte'
	let width = 10
	let height = 10
	export let ships = [
		{ size: 4, cnt: 2 },
		{ size: 3, cnt: 3 },
		{ size: 2, cnt: 3 }
	]
	export let colorScheme = ['#fff', '#0ea5e9', '#bae6fd']
	export let playerId = 'player'

	let map = []

	function init() {
		map = Array(height)
			.fill()
			.map(() => Array(width).fill(0))
	}

	function createShip(size) {
		const ship = []
		const dir = Math.floor(Math.random() * 2)
		let x = Math.floor(Math.random() * (height - size + 1))
		let y = Math.floor(Math.random() * (width - size + 1))

		for (let i = 0; i < size; i++) {
			ship.push([x, y])
			if (dir === 0) x++
			else y++
		}

		return ship
	}

	function isRegionFree(ship) {
		return ship.every(([x, y]) => map[x][y] === 0)
	}

	function placeShip(ship) {
		const [startX, startY] = ship[0]
		const [finX, finY] = ship[ship.length - 1]

		for (let x = startX - 1; x <= finX + 1; x++) {
			for (let y = startY - 1; y <= finY + 1; y++) {
				if (x >= 0 && y >= 0 && x < height && y < width) {
					map[x][y] =
						x >= startX && x <= finX && y >= startY && y <= finY ? 1 : 2
				}
			}
		}
	}

	function arrange(ships) {
		const newMap = structuredClone(map)

		for (const { size, cnt } of ships) {
			let placed = 0
			let overflow = 0

			while (placed < cnt) {
				const ship = createShip(size)

				if (isRegionFree(ship)) {
					placeShip(ship)
					placed++
				}

				overflow++

				if (overflow > 100) {
					throw new Error(
						'The number of ships is too high or dimensions of board are too small'
					)
				}
			}
		}

		return newMap
	}

	function run() {
		init()
		arrange(ships)
		console.log({ map })
	}

	onMount(() => {
		init()
	})
</script>

<div id={playerId}>
	<table>
		{#each map as row, i}
			<tr>
				{#each row as cell, j}
					<td style="background-color: {colorScheme[cell]}"> &nbsp; </td>
				{/each}
			</tr>
		{/each}
	</table>
</div>
<div class="flex justify-center py-2 gap-1">
	<button class="btn" on:click={run}>Run</button>
</div>

<style>
	table {
		border-collapse: collapse;
	}
	td {
		width: 40px;
		height: 40px;
		border: 1px solid #000;
	}
</style>
