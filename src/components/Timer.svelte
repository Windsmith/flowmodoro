<script>
	export let timerRunning = false;
	export let position;

	let timerStyle = {};
	$: {
		if (position == 'first') {
			timerStyle = {
				inputStyle: 'text-gray-200 text-6xl text-center bg-gray-900 w-3/12',
				spaceStyle: 'text-gray-200 text-6xl text-center'
			};
		} else if (position == 'second') {
			timerStyle = {
				inputStyle: 'text-gray-400 text-5xl text-center bg-gray-900 w-2/12',
				spaceStyle: 'text-gray-400 text-5xl mx-1 text-center'
			};
		} else if (position == 'third') {
			timerStyle = {
				inputStyle: 'text-gray-500 text-4xl text-center bg-gray-900 w-2/12',
				spaceStyle: 'text-gray-500 text-4xl text-center'
			};
		}
	}

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
	}

	function stopTimer() {
		clearInterval(timerid);
	}

	$: timerRunning ? startTimer() : stopTimer();
</script>

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
		class={timerStyle.inputStyle}
	/>
	<span class={timerStyle.spaceStyle}>:</span>
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
		class={timerStyle.inputStyle}
	/>
</div>
