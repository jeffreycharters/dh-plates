<script lang="ts">
	import { data } from '$lib/data';
	import Plate from '$lib/components/Plate.svelte';
	import RiderDetail from '$lib/components/RiderDetail.svelte';
	import type { Rider } from '$lib/types';

	let detailRiders: Rider[] = [];

	let y = 0;
	let eliteMen = true;
	let eliteWomen = true;
	let juniorMen = false;
	let juniorWomen = false;

	let selectedRiders = data;
	let showDetail: boolean = false;

	const getPlates = (): number[] => {
		selectedRiders = data;
		if (!eliteMen) selectedRiders = selectedRiders.filter((r) => !(r.Elite && r.Men));
		if (!eliteWomen) selectedRiders = selectedRiders.filter((r) => !(r.Elite && !r.Men));
		if (!juniorMen) selectedRiders = selectedRiders.filter((r) => !(!r.Elite && r.Men));
		if (!juniorWomen) selectedRiders = selectedRiders.filter((r) => !(!r.Elite && !r.Men));
		let plates = new Set<number>();
		selectedRiders.forEach((r) => {
			plates.add(r.CurrentRank as number);
		});
		return [...plates];
	};

	let displayPlates = getPlates();

	const handleShowToggle = () => {
		displayPlates = getPlates();
		console.log(displayPlates.length);
	};

	const handleShow = (e: CustomEvent) => {
		y = e.detail.y;
		detailRiders = selectedRiders.filter((r) => r.CurrentRank === e.detail.plate);
		if (!eliteMen) detailRiders = detailRiders.filter((r) => !(r.Elite && r.Men));
		if (!eliteWomen) detailRiders = detailRiders.filter((r) => !(r.Elite && !r.Men));
		if (!juniorMen) detailRiders = detailRiders.filter((r) => !(!r.Elite && r.Men));
		if (!juniorWomen) detailRiders = detailRiders.filter((r) => !(!r.Elite && !r.Men));
		if (!!detailRiders) showDetail = true;
	};

	const hideDetail = () => {
		showDetail = false;
	};
</script>

<svelte:head>
	<title>Jeffrey's DH Plate Identifier</title>
</svelte:head>

<h1 class="text-xl text-center font-semibold text-slate-800">UCI DH Who's who?</h1>

<h2 class="text-lg text-center text-gray-600 mb-2">
	Mont Ste Anne 2022 by <span class="font-bold">Plate Number</span>
</h2>

<div class="grid grid-cols-2 mx-auto max-w-screen-sm text-center mb-4 gap-y-3">
	<div>
		<input
			type="checkbox"
			id="elite-men"
			bind:checked={eliteMen}
			on:change={handleShowToggle}
		/><label for="elite-men">Elite Men</label>
	</div>
	<div>
		<input
			type="checkbox"
			id="elite-women"
			bind:checked={eliteWomen}
			on:change={handleShowToggle}
		/><label for="elite-women">Elite Women</label>
	</div>
	<div>
		<input
			type="checkbox"
			id="junior-men"
			bind:checked={juniorMen}
			on:change={handleShowToggle}
		/><label for="junior-men">Junior Men</label>
	</div>
	<div>
		<input
			type="checkbox"
			id="junior-women"
			bind:checked={juniorWomen}
			on:change={handleShowToggle}
		/><label for="junior-women">Junior Women</label>
	</div>
</div>

<div class="grid grid-cols-5 gap-1 relative max-w-screen-sm w-11/12 mx-auto justify-items-center">
	<div class="opacity-0">Hi</div>
	{#each displayPlates as plate}
		<Plate plateNumber={plate} on:showPlate={handleShow} on:hideDetail={hideDetail} />
	{/each}
</div>
{#if showDetail && detailRiders.length > 0}
	<RiderDetail riders={detailRiders} {y} />
{/if}

<div class="mb-12 max-w-screen-sm w-11/12 mx-auto">
	<h2 class="text-lg font-semibold my-4">Details</h2>

	<div class="my-4">Add/remove categories with the buttons at the top. Tell your friends!</div>

	<div class="my-4">Comments and feedback to jeff.charters at pm.me. Don't be mean. I am <a href="https://jeffdev.ca" class="underline text-green-600">looking
	for web development work</a>, get in touch and we'll talk.</div>

	<div class="my-4">
		Limited functionality thus far, I know. But press (on a screen) or click-and-hold on a
		laptop/desktop to display the riders associated with each plate. Data gathered from the UCI
		official website after the race at Snowshoe, July 2022.
	</div>

	<div class="my-4">
		View code on <a
			href="https://github.com/jeffreycharters/dh-plates"
			class="underline text-green-600">Github</a
		>.
	</div>
</div>

<style>
	input[type='checkbox'] {
		height: 0;
		width: 0;
		opacity: 0;
	}

	input[type='checkbox'] + label {
		@apply px-4 py-1 mx-1 text-gray-400 border rounded-md;
	}
	input[type='checkbox']:checked + label {
		@apply border-emerald-600 border text-white bg-emerald-600;
	}
</style>
