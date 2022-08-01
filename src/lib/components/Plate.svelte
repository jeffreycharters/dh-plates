<script lang="ts">
	export let plateNumber: number;
	import { createEventDispatcher } from 'svelte';

	let x: number;
	let y: number;

	const dispatch = createEventDispatcher();

	const touchHandler = (e: TouchEvent) => {
		x = e.targetTouches[0].clientX;
		y = e.targetTouches[0].clientY;
		dispatchShow(x, y);
	};

	const clickHandler = (e: MouseEvent) => {
		x = e.clientX;
		y = e.clientY;
		dispatchShow(x, y);
	};

	const hideHandler = () => {
		dispatch('hideDetail');
	};

	const dispatchShow = (x: number, y: number) => {
		dispatch('showPlate', {
			plate: plateNumber,
			x,
			y
		});
	};
</script>

<div
	class="select-none cursor-pointer border-2 border-slate-600 bg-slate-100 text-sm rounded-md px-4 py-1  font-bold "
	on:mousedown={clickHandler}
	on:mouseup={hideHandler}
	on:touchstart={touchHandler}
	on:touchend={hideHandler}
>
	{plateNumber}
</div>
