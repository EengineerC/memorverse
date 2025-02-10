<script lang="ts">
	import { goto } from '$app/navigation';
	import Description from './Description.svelte';
	import { inputText } from './state.svelte';
</script>

<main class="app-container">
	<div class="hero-section">
		<h1>MemorVerse</h1>
		<p class="subtitle">Memorize Scripture Through Interactive Practice</p>
	</div>

	<div class="content-wrapper">
		<div class="input-panel">
			<p class="panel-description">
				MemorVerse is a Bible memory app that helps you memorize scripture in an interactive way.
				Test your memory by filling in missing words from Bible verses or using speech recognition
				to recall verses. Customize your experience with features like showing the first letter of
				each word or adjusting how many words are hidden at once. Enter your bible verse below and
				start memorizing!
			</p>

			<textarea bind:value={inputText.text} placeholder="Enter your Bible verse here..."></textarea>
			<button class="btn primary" onclick={() => goto('/memorize')}> Start Memorizing </button>
		</div>
		<div class="description-panel">
			<Description />
		</div>
	</div>
</main>

<style>
	.app-container {
		min-height: 100vh;
		background: linear-gradient(135deg, #f8fafc 0%, #e2e8f0 100%);
		padding: 2rem 1rem;
		margin-top: 4rem;
	}

	.hero-section {
		text-align: center;
		margin-bottom: 3rem;
		padding-top: 2rem;
	}

	h1 {
		font-size: 3rem;
		color: #2c5282;
		margin-bottom: 0.5rem;
		font-weight: 700;
	}

	.subtitle {
		font-size: 1.25rem;
		color: #4a5568;
	}

	.content-wrapper {
		display: flex;
		justify-content: space-between;
		max-width: 1200px;
		margin: 0 auto;
		padding: 2rem;
		gap: 2rem;
	}

	.description-panel {
		flex: 1;
		background: white;
		border-radius: 16px;
		padding: 2.5rem;
		box-shadow: 0 4px 20px rgba(0, 0, 0, 0.06);
	}

	.input-panel {
		flex: 1;
		background: white;
		border-radius: 16px;
		padding: 2.5rem;
		box-shadow: 0 4px 20px rgba(0, 0, 0, 0.06);
		display: flex;
		flex-direction: column;
		gap: 1.5rem;
	}

	.panel-description {
		font-size: 1.2rem;
		color: #4a5568;
		margin-bottom: 1rem;
	}

	textarea {
		width: 100%;
		height: 55.1%;
		padding: 1.5rem;
		font-size: 1.1rem;
		line-height: 1.6;
		border: 2px solid #e2e8f0;
		border-radius: 12px;
		resize: vertical;
		transition: all 0.2s;
	}
	.btn {
		padding: 0.75rem 1.5rem;
		border: none;
		border-radius: 8px;
		font-size: 1rem;
		font-weight: 500;
		cursor: pointer;
		transition: all 0.2s;
	}

	.btn:hover {
		transform: translateY(-2px);
	}

	.btn.primary {
		background: #2c5282;
		color: white;
		box-shadow: 0 4px 12px rgba(44, 82, 130, 0.2);
	}

	.btn.primary:hover {
		background: #2b6cb0;
	}

	@media (max-width: 768px) {
		.content-wrapper {
			flex-direction: column;
			padding: 1rem;
		}

		h1 {
			font-size: 2.5rem;
		}

		.input-panel {
			padding: 1.5rem;
		}
	}
</style>
