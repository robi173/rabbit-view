<script>
	import '../app.css';
	import { store } from '$lib/store.svelte.js';
	import favicon from '$lib/assets/favicon.svg';

	let { children } = $props();
	let theme = $state('halloween');

	function saveTheme() {
		localStorage.setItem('theme', theme);
	}

	// unser "Konstruktor" (lifecycle hook) - läuft jedesmal, wenn die Seite bzw. die Komponente geladen wird:
	$effect(() => {
		store.listRabbits();
		theme = localStorage.getItem("theme");
		$inspect('🐰: ', store.rabbits);
	});
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>

<div data-theme={theme} class=" min-h-screen p-2">
	<nav class="flex items-center justify-between">
		<a href="/" class="text-4xl">🐰</a>
	<div>
		<button class="btn" onclick={saveTheme}>Save Theme!</button>
		<select class="select w-[150px]" bind:value={theme}>
		<option disabled selected>Pick a theme</option>
		<option value="light">light</option>
		<option value="dark">dark</option>
		<option value="cyberpunk">cyberpunk</option>
		<option value="valentine">valentine</option>
		<option value="halloween">halloween</option>
	</select>
	</div>
</nav>
<main class="flex flex-col items-center justify-center">
	{@render children?.()}
</main>
</div>
