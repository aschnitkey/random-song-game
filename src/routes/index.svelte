<script>
	import { musicData } from '../../static/musicList';
	export let band = '';
	export let album = '';
	export let albumCover = '';
	export let song = '';
	let hasLoadedSong = false;

	const getRandomAlbum = () => {
		const randomNum = Math.floor(Math.random() * musicData.length + 1);
		const ranArtist = musicData[randomNum].Artist.toLowerCase()
			.replaceAll(' ', '_')
			.replaceAll("'", '%27')
			.replaceAll('&', '%26')
			.replaceAll(',', '%2C');
		const ranAlbum = musicData[randomNum].Album.toLowerCase()
			.replaceAll(' ', '_')
			.replaceAll("'", '%27')
			.replaceAll('&', '%26')
			.replaceAll(',', '%2C');
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

		try {
			const res = await fetch(`${url}/${apikey}/searchalbum.php?s=${artistName}&a=${albumName}`);
			if (!res.ok) {
				band = artistName;
				album = albumName;
				throw new Error(`Response Error! Issue finding ${band} - ${album}`, {
					cause: res.statusText
				});
			}
			const data = await res.json();
			let albumData = data.album[0];
			band = albumData.strArtist;
			album = albumData.strAlbum;
			albumId = albumData.idAlbum;
			albumCover = albumData.strAlbumThumb;
			console.log(`albumId: ${albumId}`);
		} catch (err) {
			alert(err);
		}
		try {
			const songRes = await fetch(`${url}/${apikey}/track.php?m=${albumId}`);
			if (!songRes.ok) {
				throw new Error(`Response Error! Issue finding ${band} - ${album}`, {
					cause: songRes.statusText
				});
			}
			const songData = await songRes.json();
			const randomNum = Math.floor(Math.random() * songData.track.length + 1);
			console.log(randomNum);
			song = songData.track[randomNum].strTrack;
		} catch (err) {
			band = artistName;
			album = albumName;
			alert(err);
		}
	};

	const handleClick = () => {
		const { artist, album } = getRandomAlbum();
		updateSong(artist, album);
		hasLoadedSong = true;
	};
</script>

<div class="container">
	<h1 class="logo text-3xl font-bold underline uppercase mb-5">Today's Random Song</h1>
	{#if song}
		{#if albumCover}
			<img class="max-w-sm object-cover rounded-sm mb-2" src={albumCover} alt={album} />
		{/if}
		<p>{band}</p>
		<p>{album}</p>
		<p>{song}</p>
	{/if}
	{#if !song && hasLoadedSong}
		<p>There was a problem loading your track. Please try again!</p>
		<p>Error Resided with: {band} - {album}</p>
	{/if}
	<button on:click={handleClick} class="px-5 py-3 my-5 bg-gray-900 color-white">Get Song</button>
</div>
