<script lang="ts" context="module">
	class Building {
		width: number;
		length: number;
		height: number;
		color: number;
		constructor(c: number, w?: number, l?: number, h?: number) {
			this.width = w ?? 1;
			this.length = l ?? 1;
			this.height = h ?? 0;
			this.color = c;
		}
	}

	function generateRandomGaussian(mean: number, stdDev: number, min: number): number {
		let u1 = 0,
			u2 = 0;
		while (u1 === 0) u1 = Math.random(); // Ensure u1 is not zero
		while (u2 === 0) u2 = Math.random(); // Ensure u2 is not zero

		const z0 = Math.sqrt(-2.0 * Math.log(u1)) * Math.cos(2.0 * Math.PI * u2);
		return min + mean + stdDev * z0;
	}

	function CreateTestBuildings(
		count: number,
		lengthMean: number,
		lengthSTD: number,
		widthMean: number,
		widthSTD: number,
		heightMean: number,
		heightSTD: number
	): Building[] {
		let buildings: Building[] = [];
		for (let i = 0; i < count; i++) {
			buildings.push(
				new Building(
					i,
					Math.abs(Math.floor(generateRandomGaussian(widthMean, widthSTD, 1))),
					Math.abs(Math.floor(generateRandomGaussian(lengthMean, lengthSTD, 1))),
					Math.abs(Math.floor(generateRandomGaussian(heightMean, heightSTD, 0)))
				)
			);
		}
		return buildings;
	}
</script>

<script lang="ts">
	import { onMount } from 'svelte';

	var x: number = 40;
	var y: number = 30;
	var grid: number[][] = [];
	var buildings: Building[] = [];

	onMount(() => {
		InitializeGrid();
		buildings = CreateTestBuildings(10, 2, 1, 2, 1, 0, 0);
		console.log(buildings);
	});

	function InitializeGrid() {
		grid = [];
		for (let i = 0; i < x; i++) {
			let row = [];
			for (let j = 0; j < y; j++) {
				row.push(-1);
			}
			grid.push(row);
		}
	}
</script>

<div class="flex flex-row">
	<div class="flex w-auto">
		<div class="flex flex-col">
			{#each buildings as building}
				<div class="flex w-full">
					<p>
						{`${building.color}: l:${building.length} w:${building.width} h:${building.height}`}
					</p>
				</div>
			{/each}
		</div>
	</div>

	<div class="flex w-9">
		<table class="" id="display">
			{#each grid as row, r}
				<tr id={`display-row-${r}`}>
					{#each row as col, c}
						<td
							id={`display-cell-${r}-${c}`}
							class={`border-2 border-black m-3 p-3`}
						>{col}</td>
					{/each}
				</tr>
			{/each}
		</table>
	</div>
</div>
