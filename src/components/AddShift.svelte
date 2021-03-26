<script lang="ts">
	interface IShift {
		start: string;
		end: string;
	}

	export let availableShifts: IShift[];
	export let addShift: Function;
	let shiftIndex: number = 0;
	let error: string | null = null;

	const submitShift = (): void => {
		const isSuccess: boolean = addShift(
			availableShifts[shiftIndex]
		);
		if (!isSuccess) {
			error =
				"This shift overlaps with another shift in your schedule.";
		}
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
