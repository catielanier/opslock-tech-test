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
	const addShift = (shift: IShift): boolean => {
		const isValidShift: boolean = verifyShift(shift);
		return isValidShift;
	};

	const verifyShift = (shift: IShift, index: number = 0): void => {
		if (index === usershifts.length) {
			const shifts = [...usershifts];
			shifts.push(shift);
			shifts.sort(
				(a, b) =>
					parseInt(a.start, 10) -
					parseInt(b.start, 10)
			);
			usershifts = shifts;
			return;
		}
		const startShift: number = parseInt(
			usershifts[index].start,
			10
		);
		const endShift: number = parseInt(usershifts[index].end, 10);
		const start: number = parseInt(shift.start, 10);
		const end = parseInt(shift.end, 10);
		if (
			(end > startShift && end < endShift) ||
			(start > startShift && start < endShift)
		) {
			console.log("not a valid shift");
			error =
				"This shift falls within a shift you are already working.";
			return;
		} else {
			verifyShift(shift, index + 1);
		}
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
