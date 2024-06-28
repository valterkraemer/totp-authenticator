<script lang="ts">
	let key = "";
</script>

<label>
	Secret:
	<input type="text" bind:value={key} />
</label>

<a href="#{key}">Submit</a>
