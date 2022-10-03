<script>
	import Nested from "./Nested.svelte";
	import Info from "./Info.svelte";
	import Things from './Things.svelte';
	import Inner from './Inner.svelte';

	function handleMessage(event) {
		alert(event.detail.text);
	}

	let name = "wookey";
	let src = "./static/images/jam.jpeg";

	let string = `this string contains some <strong>HTML!!!</strong>`;

	let count = 1;
	$: doubled = count * 2;
	function incrementCount() {
		count += 1;
	}

	let numbers = [1, 2, 3, 4];
	function addNumber() {
		numbers.push(numbers.length + 1);
	}
	$: sum = numbers.reduce((t, n) => t + n, 0);

	const pkg = {
		name: 'svelte',
		version: 3,
		speed: 'blazing',
		website: 'https://svelte.dev'
	};

	let user = { loggedIn: false };
	function toggle() {
		user.loggedIn = !user.loggedIn;
	}

	let x = 7;

	let cats = [
		{ id: 'J---aiyznGQ', name: 'Keyboard Cat' },
		{ id: 'z_AbfPXTKms', name: 'Maru' },
		{ id: 'OUtn3pvWmpg', name: 'Henri The Existential Cat' }
	];

	let things = [
		{ id: 1, name: 'apple' },
		{ id: 2, name: 'banana' },
		{ id: 3, name: 'carrot' },
		{ id: 4, name: 'doughnut' },
		{ id: 5, name: 'egg' },
	];

	function handleClick() {
		things = things.slice(1);
	}

	async function getRandomNumber() {
		const res = await fetch(1);
		const text = await res.text();

		if (res.ok) {
			return text;
		} else {
			throw new Error(text);
		}
	}

	let promise = getRandomNumber();

	function handleClick2() {
		promise = getRandomNumber();
	}

	let m = { x: 0, y: 0 };
	function handleMousemove(event) {
		m.x = event.clientX;
		m.y = event.clientY;
	}
</script>

<h1>Hello {name}!</h1>

<img {src} alt="jam" width="100px" />

<p>{@html string}</p>

<button on:click={incrementCount}
	>click count {count} {count === 1 ? "time" : "times"}</button
>
<p>{count} doubled is {doubled}</p>

<p>sum is {sum}</p>
<button on:click={addNumber}>Add a number</button>

<Nested answer={42} /><Nested />

<style>
	p {
		color: purple;
		font-family: "Comic Sans MS", cursive;
		font-size: 2em;
	}

	div { width: 100%; height: 100%; }
</style>

<Info name={pkg.name} version={pkg.version} speed={pkg.speed} website={pkg.website}/>

{#if user.loggedIn}
	<button on:click={toggle}>
		Log out
	</button>
{/if}

{#if !user.loggedIn}
	<button on:click={toggle}>
		Log in
	</button>
{/if}

{#if user.loggedIn}
	<button on:click={toggle}>
		Log out
	</button>
{:else}
	<button on:click={toggle}>
		Log in
	</button>
{/if}

{#if x > 10}
	<p>{x} is greater than 10</p>
{:else if 5 > x}
	<p>{x} is less than 5</p>
{:else}
	<p>{x} is between 5 and 10</p>
{/if}

<h1>The Famous Cats of YouTube</h1>
<ul>
	{#each cats as { id, name }, i}
		<li><a target="_blank" href="https://www.youtube.com/watch?v={id}">
			{i + 1}: {name}
		</a></li>
	{/each}
</ul>

<button on:click={handleClick}>
	Remove first thing
</button>

{#each things as thing (thing.id) }
	<Things name={thing.name}/>
{/each}


{#await promise}
	<p>...waiting</p>
{:then number}
	<p>The number is {number}</p>
{:catch error}
	<p style="color: red">{error.message}</p>
{/await}

<div on:mousemove={handleMousemove}>
	The mouse position is {m.x} x {m.y}
</div>

<Inner on:message={handleMessage}/> 