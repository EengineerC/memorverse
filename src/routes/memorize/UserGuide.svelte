<script lang="ts">
	export let isOpen = true;
</script>

<div class="guide-container {isOpen ? 'open' : ''}">
	<button class="toggle-btn" onclick={() => (isOpen = !isOpen)}>
		{isOpen ? 'Hide Guide' : 'Show Guide'}
		<span class="arrow {isOpen ? 'up' : 'down'}">▼</span>
	</button>

	{#if isOpen}
		<div class="guide-content">
			<div class="guide-section">
				<h3>How to Use MemorVerse</h3>
				<div class="steps">
					<div class="step">
						<span class="step-number">1</span>
						<div class="step-content">
							<h4>Adjust Word Hiding</h4>
							<p>
								Use the slider to control how many words are hidden. Start with fewer hidden words
								and gradually increase the challenge.
							</p>
						</div>
					</div>

					<div class="step">
						<span class="step-number">2</span>
						<div class="step-content">
							<h4>Practice Options</h4>
							<p>
								Choose between typing or using voice recognition. Toggle "Show First Letters" for an
								extra hint when needed.
							</p>
						</div>
					</div>

					<div class="step">
						<span class="step-number">3</span>
						<div class="step-content">
							<h4>Test Your Memory</h4>
							<p>
								Type or speak the complete verse, including hidden words. Correct words will appear
								in bold, mistakes in red.
							</p>
						</div>
					</div>
				</div>

				<div class="tips">
					<h4>Tips for voice recognition:</h4>
					<ul>
						<li>Voice recognition can be picky but if you follow these steps it will work</li>
						<li>Use Google Chrome. It works in Safari but is much faster in Chrome</li>
						<li>
							Pause at punctuation marks. The speech to text system takes longer to process
							punctuation.
						</li>
						<li>
							Remember it will overwrite anything that is wrong if you pause and start speaking
							again.
						</li>
					</ul>
				</div>
			</div>
		</div>
	{/if}
</div>

<style>
	.guide-container {
		background: white;
		border-radius: 16px;
		margin-bottom: 2rem;
        margin-top: 1rem;
		box-shadow: 0 4px 20px rgba(0, 0, 0, 0.06);
		overflow: hidden;
	}

	.toggle-btn {
		width: 100%;
		padding: 1rem;
		background: #2c5282;
		color: white;
		border: none;
		font-size: 1rem;
		font-weight: 500;
		cursor: pointer;
		display: flex;
		justify-content: center;
		align-items: center;
		gap: 0.5rem;
		transition: background-color 0.2s;
	}

	.toggle-btn:hover {
		background: #2b6cb0;
	}

	.arrow {
		display: inline-block;
		transition: transform 0.3s ease;
	}

	.arrow.up {
		transform: rotate(180deg);
	}

	.guide-content {
		padding: 1.5rem;
	}

	.guide-section h3 {
		color: #2c5282;
		font-size: 1.5rem;
		margin-bottom: 1.5rem;
		text-align: center;
	}

	.steps {
		display: flex;
		flex-direction: column;
		gap: 1.5rem;
		margin-bottom: 2rem;
	}

	.step {
		display: flex;
		gap: 1rem;
		align-items: flex-start;
	}

	.step-number {
		background: #2c5282;
		color: white;
		width: 2rem;
		height: 2rem;
		border-radius: 50%;
		display: flex;
		align-items: center;
		justify-content: center;
		font-weight: bold;
		flex-shrink: 0;
	}

	.step-content {
		flex: 1;
	}

	.step-content h4 {
		color: #2c5282;
		font-size: 1.1rem;
		margin-bottom: 0.5rem;
	}

	.step-content p {
		color: #4a5568;
		line-height: 1.5;
		margin: 0;
	}

	.tips {
		background: #f8fafc;
		padding: 1.5rem;
		border-radius: 12px;
		margin-top: 2rem;
	}

	.tips h4 {
		color: #2c5282;
		font-size: 1.1rem;
		margin-bottom: 1rem;
	}

	.tips ul {
		list-style-type: none;
		padding: 0;
		margin: 0;
	}

	.tips li {
		color: #4a5568;
		margin-bottom: 0.75rem;
		padding-left: 1.5rem;
		position: relative;
	}

	.tips li:before {
		content: '•';
		color: #2c5282;
		position: absolute;
		left: 0;
	}

	@media (max-width: 768px) {
		.guide-container {
			margin: 1rem;
		}

		.guide-content {
			padding: 1rem;
		}

		.step {
			flex-direction: column;
		}

		.step-number {
			margin-bottom: 0.5rem;
		}
	}
</style>
