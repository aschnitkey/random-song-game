<script>
	import SongContainer from '../components/SongContainer.svelte';
	import { musicData } from '../../static/musicList';
	import Button from '../components/Button.svelte';
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
		setTimeout(() => {
			hasLoadedSong = true;
		}, 1000);
	};
</script>

<div class="container">
	<SongContainer {band} {album} {albumCover} {song} {hasLoadedSong} />
	<Button handler={handleClick}>Get Song</Button>
</div>
