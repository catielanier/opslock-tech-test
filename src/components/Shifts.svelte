<script lang="ts">
	interface IShift {
		start: string;
		end: string;
	}
	export let shifts: IShift[];
	export let deleteShift: Function;
</script>

<style>
	ul {
		list-style-type: none;
		list-style-position: inside;
	}

	button {
		border-radius: 50%;
		padding: 10px;
		background: red;
		color: white;
		cursor: pointer;
	}
</style>

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
