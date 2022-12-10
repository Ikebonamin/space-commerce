<script>
	export let selectedPlaneta;
	export let planetaNomes;
	$: console.log('dentro do dropdwo', planetaNomes);
</script>

<!-- svelte-ignore a11y-no-onchange -->
<section class="menu-cont">
	<select class="menu" name="menu" id="menu" bind:value={selectedPlaneta} />
</section>
