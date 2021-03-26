<script lang="ts">
	interface IShift {
		start: string;
		end: string;
	}
	export let shifts: IShift[];
	export let deleteShift: Function;
</script>

<div class="shifts">
	<h2>Your Shifts:</h2>
	<ul>
		{#each shifts as shift, index}
			<li>
				{shift.start}
				-
				{shift.end}
				<button
					class="del"
					on:click={() => {
						deleteShift(index);
					}}>X</button>
			</li>
		{/each}
	</ul>
</div>
