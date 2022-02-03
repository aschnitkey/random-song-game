<script context="module">
	export async function load() {
		const randNum = Math.floor(Math.random() * 949 + 1);
		let searchTerm = 'pearl_jam';
		return { props: { band: searchTerm } };
	}
</script>

<script>
	export let band;
</script>

<div class="container">
	<h1 class="text-3xl font-bold underline">Welcome to SvelteKit</h1>
	<p>{band}</p>
</div>

<style>
</style>
