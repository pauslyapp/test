<div class="canvas">
	{#await import('$lib/components/Canvas.svelte') then c}
		<svelte:component this={c.default} />
	{/await}
</div>

<style lang="postcss">
	.canvas {
		width: 40rem;
		height: 40rem;
		border: 1px solid black;
	}
</style>
