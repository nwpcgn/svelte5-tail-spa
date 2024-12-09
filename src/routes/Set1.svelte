<script lang="ts">
	import Ship from './ship/Ship.svelte'
	let { title = 'Set 1' } = $props()
</script>

<div class="layer nwp center fix bg-base-200">
	<div>
		<Ship />
	</div>
</div>
