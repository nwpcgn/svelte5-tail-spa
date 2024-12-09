<script lang="ts">
	import daten from './daten'
	class Pokemon {
		name = $state('')
		health = $state(10)
		maxHealth = $state(20)
		battle = $state({})
		attacks = $state({})
		sprites = $state({})
		constructor({ name, health, maxHealth, battle, attacks, sprites }) {
			this.name = name
			this.health = health
			this.maxHealth = maxHealth
			this.battle = battle
			this.attacks = attacks
			this.sprites = sprites
		}

		// Methode für einen schwachen Angriff
		weakAttack() {
			return (
				this.battle.weak.damage +
				Math.floor(Math.random() * this.battle.weak.dice) +
				1
			)
		}

		// Methode für einen starken Angriff
		hardAttack() {
			return (
				this.battle.hard.damage +
				Math.floor(Math.random() * this.battle.hard.dice) +
				1
			)
		}

		// Methode für eine spezifische Attacke
		performAttack(attackName) {
			const attack = this.attacks.find((a) => a.name === attackName)
			if (!attack)
				throw new Error(
					`${this.name} kann die Attacke ${attackName} nicht einsetzen.`
				)
			return attack.damage + Math.floor(Math.random() * attack.dice) + 1
		}

		// Methode, um Schaden zu erleiden
		takeDamage(amount) {
			this.health = Math.max(0, this.health - amount)
		}

		// Methode, um den Pokémon zu heilen
		heal(amount) {
			this.health = Math.min(this.maxHealth, this.health + amount)
		}

		// Methode, um Informationen anzuzeigen
		getInfo() {
			return `${this.name} - HP: ${this.health}/${this.maxHealth}`
		}

		// Methode, um einen Sprite anzuzeigen
		getSprite(shiny = false, front = true) {
			if (front)
				return shiny ? this.sprites.front_shiny : this.sprites.front_default
			return shiny ? this.sprites.back_shiny : this.sprites.back_default
		}
	}

	// console.log(`Schwacher Angriff: ${quilava.weakAttack()}`)
	// console.log(`Starker Angriff: ${quilava.hardAttack()}`)
	// console.log(`Flame-Charge Angriff: ${quilava.performAttack('flame-charge')}`)
	// quilava.takeDamage(20)
	// console.log(quilava.getInfo())
	// console.log(`Sprite (Front, Shiny): ${quilava.getSprite(true)}`)
	let pokemons = $state([])
	let playerId = $state(0)
	pokemons = daten.map((data) => new Pokemon(data))
	let player = $derived(pokemons[playerId])
	let current = $state(0)
	const handleSelect = async (e) => {
		console.log('handleSelect')
		playerId = parseInt(e.currentTarget.dataset.id)
		// player = pokemons[playerId]
		console.log({ player })
		player.takeDamage(1)
	}
</script>

<header>
	<article class="content">
		<div role="tablist" class="tabs tabs-bordered">
			{#each ['Tab 1', 'Tab 2', 'Tab 3'] as item, i}
				<label class="tab" class:tab-active={current == i}>
					<span>{item}</span>
					<input
						type="radio"
						bind:group={current}
						name="navi-1"
						value={i}
						class="sr-only" />
				</label>
			{/each}
		</div>
	</article>
</header>
<article class="flex-1 relative">
	<section class="layer nwp left" class:active={current == 0}>
		<article class="content">
			<h2>Heroes 1</h2>
			<h2>{player && player.name ? player.name : 'Not Selected'}</h2>
		</article>
	</section>
	<section class="layer nwp right" class:active={current == 1}>
		<article class="content">
			<h2>Heroes 2</h2>
		</article>
	</section>
	<section class="layer nwp bottom" class:active={current == 2}>
		<article class="content">
			<div class="flex flex-col divide-y">
				{#each pokemons as { name, health, maxHealth, battle, attacks, sprites }, i}
					<button
						class="flex items-center text-left"
						onclick={handleSelect}
						data-id={i}>
						<div class="flex-1" class:text-info={playerId == i}>
							<h2>{name}</h2>
							<h5>{health}/{maxHealth}</h5>
						</div>
						<figure>
							<img src={pokemons[i].getSprite()} alt={name} />
						</figure>
					</button>
				{/each}
			</div>
		</article>
	</section>
</article>
