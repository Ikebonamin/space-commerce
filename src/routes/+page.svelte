<script>
	import '../app.css';
	import { onMount } from 'svelte';
	import Footer from '../lib/Footer.svelte';
	import Header from '../lib/Header.svelte';

	let selectedPlaneta = '';
	let cart = [];
	let bgdark = 'bg-[#0d0d19]';

	let planetas = [
		{
			id: 1,
			img: 'bg-[url(/jupiter.png)]',
			nome: 'JÚPITER',
			distancia: '150 Milhoes de kilômetros',
			preco: 1000000,
			qtd: 1
		},

		{
			id: 2,
			img: 'bg-[url(/marte.png)]',
			nome: 'MARTE',
			distancia: '250 Milhoes de kilômetros',
			preco: 2,
			qtd: 1
		},
		{
			id: 3,
			img: 'bg-[url(/neptune.png)]',
			nome: 'NETUNO',
			distancia: '350 Milhoes de kilômetros',
			preco: 3,
			qtd: 1
		},
		{
			id: 4,
			img: 'bg-[url(/pluto.png)]',
			nome: 'PLUTÃO',
			distancia: '650 Milhoes de kilômetros',
			preco: 4,
			qtd: 1
		},
		{
			id: 5,
			img: 'bg-[url(/saturno.png)]',
			nome: 'SATURNO',
			distancia: '450 Milhoes de kilômetros',
			preco: 5,
			qtd: 1
		},
		{
			id: 6,
			img: 'bg-[url(/venus.png)]',
			nome: 'VÊNUS',
			distancia: '750 Milhoes de kilômetros',
			preco: 6,
			qtd: 1
		}
	];

	//pega nome dos planetas para dropdown início //
	$: console.log(selectedPlaneta);
	let planetaNomes = [];
	const getNomes = () => {
		for (let nomes of planetas) {
			if (!planetaNomes.includes(nomes.nome)) {
				planetaNomes = [...planetaNomes, nomes.nome];
			}
		}
		planetaNomes = planetaNomes.sort();
	};
	getNomes();

	//pega nome dos planetas para dropdown final  //

	//funcao dropdwon nomes //
	let filterdPlaneta = [];
	const planetaByNome = () => {
		if (selectedPlaneta === 'all') {
			return (filterdPlaneta = planetas);
		}
		for (let planeta of planetas) {
			if (planeta.nome === selectedPlaneta) {
				return (filterdPlaneta = [planeta]);
			}
		}
	};

	// função para adicionar ítem no carrinho, início //

	const addToCart = (planeta) => {
		for (let item of cart) {
			if (item.id === planeta.id) {
				planeta.qtd += 1;
				cart = cart;
				return;
			}
		}
		cart = [...cart, planeta];
		console.log(cart);
	};
	// função para adicionar ítem no carrinho, final //

	const handleRange = (e) => {
		filterdPlaneta = planetas.filter((planeta) => {
			let limiteInferior = null;
			let limiteSuperior = null;

			switch (e.target.value) {
				case '1-3':
					limiteInferior = 1;
					limiteSuperior = 3;
					break;
				case '3-6':
					limiteInferior = 3;
					limiteSuperior = 6;
					break;
			}

			return planeta.preco >= limiteInferior && planeta.preco <= limiteSuperior;
		});
	};

	// funções para adicionar ou remover + ítens dos carrinhos início //

	const minusItem = (planeta) => {
		for (let item of cart) {
			if (item.id === planeta.id) {
				planeta.qtd -= 1;

				if (planeta.qtd === 0) {
					const index = cart.indexOf(planeta);
					cart.splice(index, 1);
					//cart = cart.filter((item) => item.id !== planeta.id);
				}
				cart = cart;

				return;
			}
		}
		cart = [...cart, planeta];
	};

	// funções para adicionar ou remover + ítens dos carrinhos final //

	const sort = (e) => {
		console.log('entrou aqui');
		if (e.target.value === 'desc') {
			sortDesc();
		} else if (e.target.value === 'asc') {
			sortAsc();
		}
	};

	const sortDesc = () => {
		planetas = planetas.sort((a, b) => b.preco - a.preco);
	};

	const sortAsc = () => {
		planetas = planetas.sort((a, b) => a.preco - b.preco);
	};

	sortDesc();

	// funcâo de search//
	const handleSearch = (e) => {
		filterdPlaneta = planetas.filter((planeta) => {
			return (
				planeta.nome.toLowerCase().includes(e.target.value.toLowerCase()) ||
				planeta.distancia.toLowerCase().includes(e.target.value.toLowerCase()) ||
				planeta.preco.toString().toLowerCase().includes(e.target.value.toLowerCase())
			);
		});
	};
	// reatividades iníncio //

	$: if (selectedPlaneta) {
		planetaByNome();
	}
	$: total = cart.reduce((sum, item) => sum + item.preco * item.qtd, 0);
	$: console.log('planeta nomes main app:', planetaNomes);
	$: console.log('planeta selecionado:', selectedPlaneta);
	$: console.log('planeta filtrado:', filterdPlaneta);

	// reatividades final //
	onMount(() => getNomes());
</script>

<wrapper class="w-full h-full flex flex-col items-center justify-center {bgdark} ">
	<Header />

	<div class="h-[30rem] w-[91rem] bg-auto bg-no-repeat bg-[url('/herobglarge.png')] pt-24 pl-32 ">
		<h1 class="text-4xl font-bold w-1/2 text-slate-300">Prepare-se para uma grande Aventura!</h1>
		<p class="py-6 w-1/4  text-slate-300">
			A LABENU SPACE TRAVEL traz até você a oportunidade de conhecer os planetas do nosso sistema
			solar, com segurança e conforto.
		</p>
		<button class="btn btn-secondary">começar!</button>
	</div>
	<div class="search text-slate-300 h-[5rem] flex pt-8 justify-between space-x-4 mb-8">
		<input
			on:keyup={handleSearch}
			type="text"
			placeholder="procurar"
			class="input input-bordered input-secondary w-full max-w-xs"
		/>
		<select on:change={sort} class="input input-bordered input-secondary w-full max-w-xs ">
			<option value="desc">Preço maior</option>
			<option value="asc">Preço menor</option>
		</select>

		<!-- início dropdown -->
		<section class="menu-con flex space-x-4">
			<select
				class=" btn-secondary h-12 rounded-md p-2 "
				name="menu"
				id="menu"
				bind:value={selectedPlaneta}
			>
				<option disabled select value=""> Selecione um Planeta.</option>
				<option value="all"> Mostrar todos.</option>
				{#each planetaNomes as planeta}
					<option value={planeta}>{planeta} </option>
				{/each}
			</select>
			<!-- range select -->
			<select class=" btn-secondary h-12 rounded-md p-2 " on:change={handleRange}>
				<option disabled select value=""> Selecione um range.</option>
				<option value="all"> Mostrar todos.</option>
				<option value="1-3"> 1-3</option>
				<option value="3-6"> 3-6</option>
			</select>
		</section>
	</div>
	<!-- final dropdwon -->

	<!-- carrinho de compras -->

	{#each cart as item}
		<div
			class=" flex justify-between items-center mt-4 border-2-solid border border-secondary rounded-lg shadow-lg shadow-secondary/50 p-8 w-1/3"
		>
			<div>
				<p class="text-slate-300 ">
					Você adquiriu {item.qtd} viagem(s) para
					{item.nome} por U$$ {item.preco * item.qtd}
					de dólares
				</p>
			</div>
			{#if item.qtd > 0}
				<button
					class="btn btn-active btn-secondary shadow-lg hover:scale-115 transition-all duration-300"
					on:click={() => minusItem(item)}>remover</button
				>
			{/if}
		</div>
	{/each}

	{#if cart.length != 0}
		<div class="text-slate-300 text-center">
			<h4 class="text-md font-semibold mt-6 ">
				Total da sua compra: {total}!
				<br />prepare a sua carteira, e sua roupa de Astronauta!
			</h4>
		</div>
	{/if}
	<!-- separador -->
	<!-- inicio renderização dos planetas -->
	<div class="flex w-[70rem] space-x-4 flex-wrap justify-around items-center mt-16 ">
		{#if filterdPlaneta.length > 0}
			{#each filterdPlaneta as planeta}
				<div
					class="mt-4 mb-4 h-[22rem] w-[17rem] bg-auto bg-no-repeat {planeta.img} mb-11 border-2-solid border border-secondary rounded-lg shadow-lg shadow-secondary/50"
				>
					<div
						class="nome flex align-middle items-center justify-center w-full h-[30%] bg-slate-200 opacity-50 mt-36 text-4xl font-sans font-bold text-center"
					>
						{planeta.nome}
					</div>

					<div class="distancia  text-white text-center ">{planeta.distancia}</div>
					<div class="preco flex items-center justify-center text-lg mt-1 mb1 text-white font-bold">
						{planeta.preco.toLocaleString('pt-BR', { style: 'currency', currency: 'USD' })}
					</div>

					<button
						class="btn btn-active btn-secondary shadow-lg "
						on:click={() => addToCart(planeta)}>Adicionar ao Carrinho</button
					>
				</div>
			{/each}
		{:else}
			{#each planetas as planeta}
				<div
					class="mt-4 mb-4 h-[22rem] w-[17rem] bg-auto bg-no-repeat {planeta.img} mb-11 border-2-solid border border-secondary rounded-lg shadow-lg shadow-secondary/50"
				>
					<div
						class="nome flex align-middle items-center justify-center w-full h-[30%] bg-slate-200 opacity-50 mt-36 text-4xl font-sans font-bold text-center"
					>
						{planeta.nome}
					</div>

					<div class="distancia  text-white text-center ">{planeta.distancia}</div>
					<div class="preco flex items-center justify-center text-lg mt-1 mb1 text-white font-bold">
						{planeta.preco.toLocaleString('pt-BR', { style: 'currency', currency: 'USD' })}
					</div>
					<hover class="hover:scale-115 transition-all duration-300">
						<button
							class="btn btn-active btn-secondary shadow-lg "
							on:click={() => addToCart(planeta)}>Adicionar ao Carrinho</button
						>
					</hover>
				</div>
			{/each}
		{/if}
	</div>
	<Footer />
</wrapper>
