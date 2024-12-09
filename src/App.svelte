<script lang="ts">
	import Router, { location, link } from 'svelte-spa-router'
	import Start from './routes/Start.svelte'
	import Set1 from './routes/Set1.svelte'
	import Set2 from './routes/Set2.svelte'
	const routes = {
		'/': Start,
		'/battle/1': Set1,
		'/battle/2': Set2
	}
	const nav = [
		{ name: 'Start', href: '/', icon: 'icon-start' },
		{ name: 'Ui-Docs', href: '/battle/1', icon: 'icon-char' },
		{ name: 'Sandbox', href: '/battle/2', icon: 'icon-cast' }
	]
</script>

<nav class="navbar bg-base-200">
	<div class="flex-1">
		<a href="/" class="btn btn-ghost text-xl">daisyUI</a>
	</div>
	<div class="flex-none">
		<ul class="menu menu-horizontal px-1">
			{#each nav as { name, href }}
				<li><a {href} use:link>{name}</a></li>
			{/each}
		</ul>
	</div>
</nav>
<main class="main">
	<Router {routes} />
</main>
