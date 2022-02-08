<script>
	export let band;
	export let album;
	export let song;
	export let albumCover;
	export let hasLoadedSong;
</script>

<div class="container flex flex-col items-center bg-[rgba(0,0,0,.5)] px-10 py-5 min-w-full mt-10">
	<h1 class="logo text-5xl font-bold underline uppercase mb-5">Today's Random Song</h1>
	{#if song}
		{#if albumCover}
			<img class="max-w-sm object-cover rounded-sm mb-8" src={albumCover} alt={album} />
		{/if}
		<h2 class="text-4xl mb-2">{band}</h2>
		<p class="text-xl italic">"{song}"</p>
		<p class="text-sm">from the album {album}</p>
	{/if}
	{#if !song && hasLoadedSong}
		<p>There was a problem loading your track. Please try again!</p>
		<p>Error Resided with: {band} - {album}</p>
	{/if}
	{#if !hasLoadedSong}
		<p>Push button below to get today's song</p>
	{/if}
</div>

<style>
	h1.logo {
		font-family: 'Bebas Neue', cursive;
	}
</style>
