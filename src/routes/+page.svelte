<script lang="ts">
	import { goto } from '$app/navigation';
	import Header from './Header.svelte';
	import { inputText } from './state.svelte';

	$inspect(inputText.text);
</script>

<Header />
<main>
	<div class="background">
	
		<div class="input-container">
			<textarea bind:value={inputText.text} placeholder="Enter your Bible verse here..." rows="6"
			></textarea>
			<button onclick={() => goto('/memorize')}>Start Memorizing</button>
		</div>
	</div>
</main>

<style>
	main {
		padding-top: 4rem;
	}

	.background {
		min-height: 100vh;
		background: linear-gradient(to bottom right, #f8f9ff, #e6f0ff);
		padding: 2rem;
	}

	.input-container {
		max-width: 800px;
		margin: 2rem auto;
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 1.5rem;
	}

	textarea {
		width: 100%;
		min-height: 200px;
		padding: 1rem;
		font-size: 1.2rem;
		line-height: 1.6;
		border: 2px solid #2c5282;
		border-radius: 8px;
		background: white;
		box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
		resize: vertical;
	}

	textarea:focus {
		outline: none;
		border-color: #4299e1;
		box-shadow: 0 0 0 3px rgba(66, 153, 225, 0.2);
	}

	button {
		background-color: #2c5282;
		color: white;
		border: none;
		padding: 1rem 2rem;
		font-size: 1.1rem;
		border-radius: 8px;
		cursor: pointer;
		transition:
			background-color 0.3s,
			transform 0.2s;
		box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
	}

	button:hover {
		background-color: #3182ce;
		transform: translateY(-1px);
	}
</style>
