<script>
	export let handler;
</script>

<button
	on:click={handler}
	class="rounded-md px-5 py-3 my-5 bg-gray-900 hover:bg-gray-800 color-white"><slot /></button
>
