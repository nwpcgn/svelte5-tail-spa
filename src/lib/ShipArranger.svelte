<script>
	import { onMount } from 'svelte'
	let {
		width = 10,
		height = 10,
		size = 50,
		ships = [
			{ name: 'battleship', size: 4, cnt: 1 },
			{ name: 'cruiser', size: 3, cnt: 2 },
			{ name: 'destroyer', size: 2, cnt: 3 },
			{ name: 'submarine', size: 1, cnt: 4 }
		],
		colorScheme = ['#fff', '#0ea5e9', '#bae6fd'],
		playerId = 'player'
	} = $props()

	let map = $state([])

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
		const newMap = map

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
		// console.log(map)
	}

	onMount(() => {
		init()
	})
</script>

<div id={playerId}>
	<div class="arena" style="--s: {width}; --w: {size}px; --h: {size}px;">
		{#each map as row, i}
			{#each row as cell, j}
				<button
					class="col-{cell} transition-all ease-in-out duration-300 active:opacity-40"
					onclick={() => {
						console.log({
							id: i * width + j,
							cell,
							r: i + 1,
							c: j + 1,
							i,
							j
						})
					}}><span class="sr-only">{i * width + j}</span></button>
			{/each}
		{/each}
	</div>
</div>
<div class="flex justify-center py-2 gap-1">
	<button class="btn btn-neutral" onclick={run}>Run</button>
</div>

<style>
	.arena {
		--s: 10;
		--w: 40px;
		--h: 40px;
		--color-0: rgba(255, 255, 255, 1);
		--color-1: rgba(14, 165, 233, 1);
		--color-2: rgba(186, 230, 253, 1);
		padding: 1px;
		gap: 1px;
		display: inline-grid;
		grid-template-columns: repeat(var(--s), minmax(0, 1fr));
		grid-template-rows: repeat(var(--s), minmax(0, 1fr));
		background-color: var(--gray-200);
	}
	.arena > * {
		width: var(--w);
		height: var(--h);
	}
	.col-0 {
		background-color: var(--color-0);
	}
	.col-1 {
		background-color: var(--color-1);
	}
	.col-2 {
		background-color: var(--color-2);
	}
</style>
