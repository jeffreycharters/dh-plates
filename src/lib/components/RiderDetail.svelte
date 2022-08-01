<script lang="ts">
	import type { Rider } from '$lib/types';
	import { fade } from 'svelte/transition';

	export let riders: Rider[] = [];
	export let y: number;

	const positionY = y > 200 ? 'top-2' : 'bottom-2';

	const riderClasses = (rider: Rider): string => {
		if (rider.Elite && rider.Men) return 'border-sky-700 bg-sky-50 text-sky-900';
		if (rider.Elite && !rider.Men) return 'border-emerald-700 text-emerald-700 bg-emerald-50';
		if (!rider.Elite && rider.Men) return 'border-blue-400 bg-blue-50 text-blue-400';
		return 'border-green-400 bg-green-50 text-green-400';
	};

	const riderGroup = (rider: Rider): string => {
		let group = '';
		if (rider.Elite) group += 'Elite';
		else group += 'Junior';
		if (rider.Men) group += ' Men';
		else group += ' Women';
		return group;
	};
</script>

<div
	class="fixed bg-white rounded-md border-slate-600 border py-2 px-2 select-none {positionY} left-2/4 -translate-x-2/4 w-11/12 max-w-screen-sm shadow"
	transition:fade={{ duration: 100 }}
>
	{#each riders as rider (rider.RiderName)}
		<div class="flex border-l-4 {riderClasses(rider)} pl-2 truncate gap-2 my-1">
			<div class="font-bold">
				{rider.CurrentRank}
			</div>
			<div class="font-semibold">
				{rider.RiderName}
			</div>
			<span class="text-slate-500">({riderGroup(rider)})</span>
		</div>
	{/each}
</div>
