<script lang="ts">
	interface IShift {
		start: string;
		end: string;
	}

	export let availableShifts: IShift[];
	export let addShift: Function;
	let shiftIndex: number = 0;
	export let error: string | null;
	const submitShift = (): void => {
		addShift(availableShifts[shiftIndex]);
	};
</script>

<div class="available-shifts">
	<h2>Add a Shift:</h2>
	{#if error !== null}
		<p><span class="error">Error:</span> {error}</p>
	{/if}
	<select name="shifts" id="shifts" bind:value={shiftIndex}>
		{#each availableShifts as shift, index}
			<option value={index}>
				{shift.start}
				-
				{shift.end}
			</option>
		{/each}
	</select>
	<button on:click={submitShift}>Add</button>
</div>
