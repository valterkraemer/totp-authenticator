<script lang="ts">
	import { onMount } from "svelte";
	import New from "./New.svelte";
	import Show from "./Show.svelte";

	let href = $state(location.href);

	const handleHashChange = () => {
		console.log("handleHashChange");
		href = location.href;
	};

	onMount(() => {
		window.addEventListener("hashchange", handleHashChange);

		return () => {
			window.removeEventListener("hashchange", handleHashChange);
		};
	});

	const key = $derived.by(() => {
		const url = new URL(href);
		return url.hash.slice(1);
	});
</script>

{#if key}
	<Show {key} />
{:else}
	<New />
{/if}
