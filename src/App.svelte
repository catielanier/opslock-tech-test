<script lang="ts">
	import Shifts from "./components/Shifts.svelte";
	import AddShift from "./components/AddShift.svelte";
	interface IShift {
		start: string;
		end: string;
	}
	let error: string | null = null;
	let usershifts: IShift[] = [
		{
			start: "0600",
			end: "1000",
		},
		{
			start: "1600",
			end: "2000",
		},
	];

	let availableCompanyShifts: IShift[] = [
		{
			start: "0000",
			end: "2359",
		},
		{
			start: "0600",
			end: "1800",
		},
		{
			start: "0000",
			end: "1200",
		},
		{
			start: "0600",
			end: "1200",
		},
		{
			start: "1000",
			end: "1400",
		},
		{
			start: "1800",
			end: "2359",
		},
		{
			start: "0000",
			end: "0600",
		},
		{
			start: "1200",
			end: "2359",
		},
		{
			start: "1200",
			end: "1800",
		},
	];

	const addShift = (shift: IShift): void => {
		// set the start and end times of the shift into ints
		const start: number = parseInt(shift.start, 10);
		const end: number = parseInt(shift.end, 10);
		// create an empty to house each hour of the shift, include start and end times by default
		const shiftHours: number[] = [start, end];
		// loop through the start and end times by one hour, and add each hour to the array
		for (let i = start + 100; i < end; i += 100) {
			shiftHours.push(i);
		}
		// run the verification of the shift
		verifyShift(shift, shiftHours);
	};

	// recursive function to verify that each shift the user is working doesn't conflict with the selected shift
	const verifyShift = (
		shift: IShift,
		shiftHours: number[],
		index: number = 0
	): void => {
		// if all user shifts have been checked, add the selected shift to the shifts array, then sort it by shift time
		if (index === usershifts.length) {
			const shifts = [...usershifts];
			shifts.push(shift);
			shifts.sort(
				(a, b) =>
					parseInt(a.start, 10) -
					parseInt(b.start, 10)
			);
			usershifts = shifts;
			// reset the error message in case it was previously displayed
			error = null;
			// empty return to end the function, prevent endless loop
			return;
		}
		// set the start and end times of the currently compared user shift as ints
		const startShift: number = parseInt(
			usershifts[index].start,
			10
		);
		const endShift: number = parseInt(usershifts[index].end, 10);
		// loop through each hour of the selected shift to see if it falls within the user's existing shift
		for (let i = 0; i < shiftHours.length; i++) {
			if (
				shiftHours[i] > startShift &&
				shiftHours[i] < endShift
			) {
				// if it does, set the error message and empty return to end the function.
				error =
					"This shift falls within a shift you are already working.";
				return;
			}
		}
		// if the loop through doesn't find a conflict, rerun the recursive function on the next index of the user's shifts
		verifyShift(shift, shiftHours, index + 1);
	};

	const deleteShift = (index: number): void => {
		console.log(`running ${index}`);
		const shifts = [...usershifts];
		shifts.splice(index, 1);
		usershifts = shifts;
	};
</script>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>

<main>
	<h1>Shift Manager</h1>
	<Shifts shifts={usershifts} {deleteShift} />
	<AddShift availableShifts={availableCompanyShifts} {addShift} {error} />
</main>
