<script>
	import { loop_guard } from 'svelte/internal';

	let timerRunning = false;
	let timerid;
	let minutes = 25;
	let secondCount = 0;

	let secondText = 0;

	$: if (secondCount < 10) {
		secondText = `0${secondCount}`;
	} else {
		secondText = secondCount;
	}

	function handleTimerValueChange() {
		if (secondCount == 0) {
			minutes -= 1;
			secondCount = 59;
		} else {
			secondCount -= 1;
		}
	}

	function startTimer() {
		timerid = setInterval(handleTimerValueChange, 1000);
		timerRunning = true;
	}

	function stopTimer() {
		clearInterval(timerid);
		timerRunning = false;
	}
</script>

<div class="flex flex-col justify-center h-screen">
	<div class="mb-10">
		<div class="flex flex-row justify-center">
			<input
				bind:value={minutes}
				on:input={() => {
					if (!/^\d+$/.test(minutes)) {
						minutes = minutes.slice(0, -1);
					}
				}}
				on:change={() => {
					if (minutes == '') minutes = 0;

					let intMinutes = parseInt(minutes);

					if (intMinutes > 60) {
						minutes = 60;
					} else {
						minutes = parseInt(minutes);
					}
				}}
				maxlength="2"
				class="text-gray-200 text-6xl text-center bg-gray-900 w-3/12"
			/>
			<span class="text-gray-200 text-6xl text-center">:</span>
			<input
				bind:value={secondText}
				on:input={() => {
					if (!/^\d+$/.test(secondText)) {
						secondText = secondText.slice(0, -1);
					}
				}}
				on:change={() => {
					if (secondText == '') secondText = '00';

					let intSeconds = parseInt(secondText);

					if (intSeconds >= 60 && secondCount != 59) {
						secondCount = 59;
					} else if (intSeconds >= 60 && secondCount == 59) {
						secondText = secondCount;
					} else {
						secondCount = intSeconds;
					}
				}}
				maxlength="2"
				class="text-gray-200 text-6xl text-center bg-gray-900 w-3/12"
			/>
		</div>
		<p class="text-gray-500 text-4xl text-center">5:00</p>
		<p class="text-gray-700 text-2xl text-center">30:00</p>
	</div>
	<button
		on:click={() => (timerRunning ? stopTimer() : startTimer())}
		class="text-gray-200 text-4xl border-gray-200 border-2 p-4 px-8 mx-auto rounded-2xl active:border-gray-500 active:text-gray-400"
		>Start!
	</button>
</div>
