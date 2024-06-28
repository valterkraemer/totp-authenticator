<script lang="ts">
	import { TOTP } from "$lib/totp";
	import { onMount } from "svelte";

	let { key } = $props<{ key: string }>();

	let date = $state(Date.now());

	onMount(() => {
		const interval = setInterval(() => {
			date = Date.now();
		}, 1000);

		return () => clearInterval(interval);
	});

	const item = $derived.by(() => {
		// depend on date
		date;
		return TOTP.generate(key, {
			encoding: "ascii",
		});
	});
</script>

{#await item}
	Loading
{:then value}
	<p>
		<button onclick={() => navigator.clipboard.writeText(value.otp)}
			>{value.otp}</button
		>
		({Math.floor((value.expires - date) / 1000)}s)
	</p>
{:catch error}
	<p>Error: {error.message}</p>
{/await}

<style>
	button {
		padding: 16px;
	}
</style>
