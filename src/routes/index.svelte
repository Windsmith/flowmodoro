<script>
	import Timer from '../components/Timer.svelte';

	let timerStart = false;
</script>

<div class="flex flex-col justify-center h-screen">
	<div class="mb-10">
		<Timer timerRunning={timerStart} position="first" />
	</div>
	<button
		on:click={() => (timerStart ? (timerStart = false) : (timerStart = true))}
		class="text-gray-200 text-4xl border-gray-200 border-2 p-4 px-8 mx-auto rounded-2xl active:border-gray-500 active:text-gray-400"
	>
		{timerStart ? 'Pause' : 'Start!'}
	</button>
</div>
