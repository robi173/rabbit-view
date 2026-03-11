<script>
	import '../app.css';
	import { pb, store } from '$lib/store.svelte.js';
	import favicon from '$lib/assets/favicon.svg';
	import { goto } from '$app/navigation';

	let { children } = $props();
	let theme = $state('halloween');

	function saveTheme() {
		localStorage.setItem('theme', theme);
	}

	// unser "Konstruktor" (lifecycle hook) - läuft jedesmal, wenn die Seite bzw. die Komponente geladen wird:
	$effect(() => {
		if (!pb.authStore.isValid) {
			goto('/auth');
		}
		store.listRabbits();
		if(localStorage.getItem("theme"))
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
	
		<select class="select w-[150px]" bind:value={theme}>

		<option disabled selected>Pick a theme</option>
		<option value="light" onclick={saveTheme}>light</option>
		<option value="dark" onclick={saveTheme}>dark</option>
		<option value="cyberpunk" onclick={saveTheme}>cyberpunk</option>
		<option value="valentine" onclick={saveTheme}>valentine</option>
		<option value="halloween" onclick={saveTheme}>halloween</option>
	</select>
</nav>
<main class="flex flex-col items-center justify-center min-h-screen">
	{@render children?.()}
	</main>
</div>
