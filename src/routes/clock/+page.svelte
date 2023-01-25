<script lang="ts">
	import Fullscreen from "$lib/fullscreen.svelte";
	import { onMount } from "svelte";

	let hour: string | number = 0;
	let min: string | number = 0;
	let sec: string | number = 0;

	let interval: NodeJS.Timer;
	let bgColor: string = "2e2727";
	let previousColor: string = "2e2727";
	let nextColor: string = "000";

	let eventName: string = "The Newton Musical Rehearsal";

	function updateTime() {
		const now = new Date().toTimeString().split(" ")[0].split(":");
		hour = now[0];
		min = now[1];
		sec = now[2];
	}

	onMount(() => {
		console.log("onMount", bgColor);
		updateTime();
	});

	$: {
		clearInterval(interval);
		interval = setInterval(updateTime, 1000);
	}
</script>

<Fullscreen let:onToggle>
	<div
		class={`transotion-color duration-300 ease-in-out grid justify-start px-[10rem] items-center h-screen bg-[#${bgColor}]`}
	>
		<div>
			<div class="font-bold text-[30vh] pb-1 flex justify-end text-left text-rose-600 leading-none">
				{hour}:{min}
				<div class="text-[13vh] pl-3 opacity-60 pt-[3.5vh]">{sec}</div>
			</div>
		</div>
		<h2 class="bottom-8 right-8 text-[4vh] font-medium absolute text-rose-500 opacity-60">
			{eventName}
		</h2>

		<div
			class="flex flex-col absolute top-4 right-4 opacity-0 hover:opacity-100 transition duration-500 ease-in-out "
		>
			<button
				class=" hover:scale-110 transition duration-200 ease-in-out text-[1.75rem] text-rose-400 font-bold"
				on:click={() => onToggle()}
				title="Enter / Exit Full Screen"
				><i class="bx bx-fullscreen" />
			</button>

			<button
				class=" hover:scale-110 transition duration-200 ease-in-out text-[1.75rem] text-rose-400 font-bold"
				on:click={() => {
					const input = prompt("Event Name:");
					if (!input) return alert("Aborted! New Event Name Not provided");

					eventName = input;
				}}
				title="Edit Event Name"
			>
				<i class="bx bx-edit" />
			</button>

			<button
				class="hover:scale-110 transition duration-200 ease-in-out text-[1.75rem] text-rose-400 font-bold"
				on:click={() => {
					previousColor = bgColor;
					bgColor = nextColor;
					nextColor = previousColor;
				}}
				title="toggle OLED background"
			>
				<i class="bx bx-bulb" />
			</button>
		</div>
	</div>
</Fullscreen>
