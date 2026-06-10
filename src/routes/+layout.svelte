<script>
	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" type="image/png" href="/icons/cat-icon-192x192.png" />
</svelte:head>

{@render children()}
