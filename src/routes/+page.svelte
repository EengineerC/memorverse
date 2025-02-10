<script lang="ts">
	import { goto } from '$app/navigation';
	import { inputText } from './state.svelte';
</script>

<main class="app-container">
	<div class="hero-section">
		<h1>MemorVerse</h1>
		<p class="subtitle">Memorize Scripture Through Interactive Practice</p>
	</div>

	<div class="description-panel">
		<p class="panel-description">
			MemorVerse is a Bible memory app that helps you memorize scripture in an interactive way. Test
			your memory by filling in missing words of Bible verses by typing or by using speech
			recognition. Customize your experience with features like showing the first letter of each
			word or adjusting how many words are hidden at once. Enter your bible verse below and start
			memorizing!
		</p>

		<div class="features">
			<div class="feature">
				<span class="icon">🎯</span>
				<h3>Interactive Practice</h3>
				<p>Test your memory with customizable word hiding</p>
			</div>
			<div class="feature">
				<span class="icon">🎤</span>
				<h3>Voice Recognition</h3>
				<p>Practice verses hands-free using speech</p>
			</div>
			<div class="feature">
				<span class="icon">⚡</span>
				<h3>Customizable</h3>
				<p>Adjust difficulty to match your learning style</p>
			</div>
		</div>
		<textarea bind:value={inputText.text} placeholder="Enter your Bible verse here..."></textarea>
		<button class="btn primary" onclick={() => goto('/memorize')}> Start Memorizing </button>
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

	.description-panel {
		width: 100%;
		background: white;
		border-radius: 16px;
		padding: 2.5rem;
		margin: 0 auto 2rem;
		box-shadow: 0 4px 20px rgba(0, 0, 0, 0.06);
		max-width: 1000px;
	}

	p {
		color: #4a5568;
		line-height: 1.7;
		font-size: 1.1rem;
		margin-bottom: 2rem;
	}

	.features {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
		gap: 2rem;
		margin-top: 3rem;
	}

	.feature {
		text-align: center;
		padding: 1.5rem;
		background: #f8fafc;
		border-radius: 12px;
		transition: transform 0.2s;
		border: 1px solid #e2e8f0;
	}

	.feature:hover {
		transform: translateY(-5px);
		box-shadow: 0 4px 12px rgba(44, 82, 130, 0.1);
	}

	.icon {
		font-size: 2rem;
		margin-bottom: 1rem;
		display: inline-block;
	}

	h3 {
		color: #2c5282;
		font-size: 1.25rem;
		margin-bottom: 0.5rem;
	}

	.feature p {
		color: #4a5568;
		font-size: 1rem;
		margin-bottom: 0;
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

	.panel-description {
		font-size: 1.2rem;
		color: #4a5568;
		margin-bottom: 1rem;
	}

	textarea {
		height: 300px;
		width: 100%;
		padding: 2rem;
		font-size: 1.1rem;
		line-height: 1.6;
		border: 2px solid #e2e8f0;
		border-radius: 12px;
		resize: vertical;
		transition: all 0.2s;
		margin-top: 2rem;
		color: #4a5568;
	}

	.btn {
		padding: 0.75rem 1.5rem;
		border: none;
		border-radius: 8px;
		font-size: 1rem;
		font-weight: 500;
		cursor: pointer;
		transition: all 0.2s;
		margin-top: 1.7rem;
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
		.description-panel {
			padding: 1.5rem;
			margin: 1rem;
		}

		.features {
			grid-template-columns: 1fr;
			gap: 1rem;
		}
	}
</style>
