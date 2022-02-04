<script context="module">
	export async function load() {
		const randNum = Math.floor(Math.random() * 949 + 1);
		const url = import.meta.env.VITE_AUDIODB_URL;
		// const apikey = import.meta.env.VITE_AUDIODB_API_KEY;
		const apikey = 2;
		let searchTerm = 'bush';
		const res = await fetch(`${url}/${apikey}/search.php?s=${searchTerm}`);
		const data = await res.json();
		return { props: { band: data.artists[0].strArtist } };
	}
</script>

<script>
	import { musicData } from '../../static/musicList';
	console.log(musicData[0]);
	export let band;

	const updateArtist = async () => {
		let searchTerm = 'deafheaven';
		const url = import.meta.env.VITE_AUDIODB_URL;
		// const apikey = import.meta.env.VITE_AUDIODB_API_KEY;
		const apikey = 2;
		const res = await fetch(`${url}/${apikey}/search.php?s=${searchTerm}`);
		const data = await res.json();
		band = data.artists[0].strArtist;
	};

	const handleClick = () => {
		updateArtist();
	};
</script>

<div class="container">
	<h1 class="logo text-3xl font-bold underline">Welcome to SvelteKit</h1>
	<p>{band}</p>
	<button on:click={handleClick} class="px-5 py-3 my-5 bg-gray-900 color-white">Deafheaven</button>
</div>
