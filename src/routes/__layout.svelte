<script>
	import '../styles/app.css';
	import Icon from '@iconify/svelte';
	import Nav from '../components/nav.svelte';
	const d = new Date();
	const year = d.getFullYear();
</script>

<svelte:head>
	<title>Random Song Game</title>
</svelte:head>
<body class="flex flex-col min-h-screen">
	<header>
		<Nav />
	</header>
	<main class="conatiner p-5 flex-1 mx-auto">
		<slot />
	</main>
	<footer class="h-28 bg-gray-800 py-5 px-10 flex items-center justify-end">
		<a
			class="flex items-center px-10 py-5 mr-0"
			href="http://https://github.com/aschnitkey/random-song-game"
		>
			<Icon icon="akar-icons:github-fill" width="30" height="30" />
			<p class="ml-2 text-sm uppercase">&copy; {year} Andrew Schnitkey</p>
		</a>
	</footer>
</body>
