<script>
	export let id;
	export let nome = '';
	export let distancia = '';
	export let preco = '';
	export let img = '';
</script>

<card
	class="mt-48 mb-8 h-[22rem] w-[17rem] bg-auto bg-no-repeat bg-[url('{img}')] border-2 border-white"
>
	<div class="id text white">{id}</div>
	<div class="nome bg-secondary">{nome}</div>
	<div class="distancia">{distancia}</div>
	<div class="preco ">{preco}</div>
	<h1 class="preco text-4xl text-white ">{preco}</h1>
</card>
