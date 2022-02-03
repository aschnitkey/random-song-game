<script>
	import '../styles/global.css';
	import Icon from '@iconify/svelte';
</script>

<svelte:head>
	<title>Random Song Game</title>
</svelte:head>
<body class="flex flex-col min-h-screen">
	<header>
		<nav class="w-100 flex justify-between items-center h-14 p-2">
			<a href="/"><h1 class="text-3xl ml-5">Random Song Game</h1></a>
			<a class="mr-5" href="/about">About</a>
		</nav>
	</header>
	<main class="conatiner p-5 flex-1">
		<slot />
	</main>
	<footer class="h-28 bg-gray-800 py-5 px-10 flex items-center justify-end">
		<Icon icon="akar-icons:github-fill" width="30" height="30" />
	</footer>
</body>
