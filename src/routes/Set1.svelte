<script lang="ts">
	import Ship from './ship/Ship.svelte'
	let { title = 'Set 1' } = $props()
</script>

<div class="layer nwp">
	<div class="content">
		<h1>{title}</h1>
	</div>
	<div class="content">
		<Ship />
	</div>
</div>
