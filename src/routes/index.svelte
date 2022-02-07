<script>
	import { musicData } from '../../static/musicList';
	export let band = '';
	export let album = '';
	export let song = '';

	const getRandomAlbum = () => {
		const randomNum = Math.floor(Math.random() * musicData.length + 1);
		const ranArtist = musicData[randomNum].Artist.toLowerCase().replaceAll(' ', '_');
		const ranAlbum = musicData[randomNum].Album.toLowerCase().replaceAll(' ', '_');
		console.log(ranAlbum, ranArtist);
		return {
			artist: ranArtist,
			album: ranAlbum
		};
	};

	const updateSong = async (artistName, albumName) => {
		let albumId;
		const url = import.meta.env.VITE_AUDIODB_URL;
		const apikey = import.meta.env.VITE_AUDIODB_API_KEY;
		console.log(`URL: ${url}/${apikey}/searchalbum.php?s=${artistName}&a=${albumName}`);
		const res = await fetch(`${url}/${apikey}/searchalbum.php?s=${artistName}&a=${albumName}`);
		const data = await res.json();
		band = data.album[0].strArtist;
		album = data.album[0].strAlbum;
		albumId = data.album[0].idAlbum;
		console.log(`albumId: ${albumId}`);
		const songRes = await fetch(`${url}/${apikey}/track.php?m=${albumId}`);
		const songData = await songRes.json();
		const randomNum = Math.floor(Math.random() * songData.track.length + 1);
		console.log(randomNum);
		song = songData.track[randomNum].strTrack;
	};

	const handleClick = () => {
		const { artist, album } = getRandomAlbum();
		updateSong(artist, album);
	};
</script>

<div class="container">
	<h1 class="logo text-3xl font-bold underline uppercase">Today's Random Song</h1>
	<p>{band}</p>
	<p>{album}</p>
	<p>{song}</p>
	<button on:click={handleClick} class="px-5 py-3 my-5 bg-gray-900 color-white">Get Song</button>
</div>
