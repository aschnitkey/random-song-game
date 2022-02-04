<!-- <script context="module">
	export async function load() {
		// const randNum = Math.floor(Math.random() * 949 + 1);
		// const url = import.meta.env.VITE_AUDIODB_URL;
		// // const apikey = import.meta.env.VITE_AUDIODB_API_KEY;
		// const apikey = 2;
		// let searchTerm = 'bush';
		// const res = await fetch(`${url}/${apikey}/search.php?s=${searchTerm}`);
		// const data = await res.json();
		// return { props: { band: data.artists[0].strArtist } };
		return { props: { band: '' } };
	}
</script> -->
<script>
	import { musicData } from '../../static/musicList';
	export let band = '';
	export let album = '';
	export let song = '';

	const getRandomAlbum = () => {
		const randomNum = Math.floor(Math.random() * musicData.length + 1);
		const ranArtist = musicData[randomNum].Artist;
		const ranAlbum = musicData[randomNum].Album;
		return {
			artist: ranArtist,
			album: ranAlbum
		};
	};

	const updateArtist = async (searchTerm) => {
		const url = import.meta.env.VITE_AUDIODB_URL;
		// const apikey = import.meta.env.VITE_AUDIODB_API_KEY;
		const apikey = 2;
		const res = await fetch(`${url}/${apikey}/search.php?s=${searchTerm}`);
		const data = await res.json();
		band = data.artists[0].strArtist;
	};

	const updateSong = async (artistName, albumName) => {
		let albumId;
		const url = import.meta.env.VITE_AUDIODB_URL;
		const apikey = import.meta.env.VITE_AUDIODB_API_KEY;
		const res = await fetch(`${url}/${apikey}/searchalbum.php?s=${artistName}&a=${albumName}`);
		const data = await res.json();
		albumId = data.album[0].idAlbum;
		const songRes = await fetch(`${url}/${apikey}/track.php?m=${albumId}`);
		const songData = await songRes.json();
		const randomNum = Math.floor(Math.random() * songData.track.length + 1);
		console.log(randomNum);
		song = songData.track[randomNum].strTrack;
	};

	const handleClick = () => {
		const searchTerm = getRandomAlbum();
		updateArtist(searchTerm.artist);
		album = searchTerm.album;
		updateSong(searchTerm.artist, searchTerm.album);
	};
</script>

<div class="container">
	<h1 class="logo text-3xl font-bold underline uppercase">Today's Random Song</h1>
	<p>{band}</p>
	<p>{album}</p>
	<p>{song}</p>
	<button on:click={handleClick} class="px-5 py-3 my-5 bg-gray-900 color-white">Get Song</button>
</div>
