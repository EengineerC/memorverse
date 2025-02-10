<script lang="ts">
	import { goto } from '$app/navigation';
	import { inputText } from '../state.svelte';
	import Switch from './Switch.svelte';
	import UserGuide from './UserGuide.svelte';

	let numberToHide = $state(0);
	let hiddenIndices: number[] = $state([]);
	let show = $state(false);
	let showFirstLetterOnly = $state(false);
	let guessText = $state('');
	let isListening = $state(false);
	let recognition: SpeechRecognition | null = null;
	let lastCorrectIndex = 0;
	let text = inputText.text;
	const words = text.split(' ');
	const numberOfWords = words.length;
	let initialized = false;

	$effect(() => {
		for (let i = 0; i < text.length; i++) {
			if (!/^[a-zA-Z]+$/.test(text[i])) {
				//this is to deal with punctuation when speaking.
				// it automatically fills forward punctuation
				guessText = guessText.slice(0, i) + text[i] + guessText.slice(i + 1);
				//marks whatever the last non-letter ther is to the last correct index
				lastCorrectIndex = i;
			}
			//if it doesn't match make it so everytime i talk it rewrites over the last place
			if (guessText[i] !== text[i].toLowerCase()) {
				break;
			}
		}
	});

	function initializeSpeechRecognition() {
		initialized = true;
		if ('webkitSpeechRecognition' in window) {
			recognition = new webkitSpeechRecognition();
			recognition.continuous = true;
			recognition.interimResults = true;

			recognition.onresult = (event) => {
				const result = event.results[event.results.length - 1];
				if (result.isFinal) {
					guessText = guessText.slice(0, lastCorrectIndex) + result[0].transcript.toLowerCase();
				}
			};

			recognition.onerror = (event) => {
				console.error('Speech recognition error:', event.error);
				isListening = false;
			};

			recognition.onend = () => {
				isListening = false;
			};
		}
	}

	$effect(() => {
		if (recognition) {
			recognition.stop();
		}
	});

	function toggleSpeechRecognition() {
		if (!initialized) {
			initializeSpeechRecognition();
		}
		if (!recognition) {
			alert('Speech recognition is not supported in your browser.');
			return;
		}
		if (isListening) {
			recognition.stop();
			isListening = false;
		} else {
			recognition.start();
			isListening = true;
		}
	}

	function updateHiddenWords() {
		while (hiddenIndices.length < numberToHide && hiddenIndices.length < numberOfWords) {
			let randomIndex = Math.floor(Math.random() * numberOfWords);
			if (!hiddenIndices.includes(randomIndex)) {
				hiddenIndices = [...hiddenIndices, randomIndex];
			}
		}
		while (hiddenIndices.length > numberToHide) {
			hiddenIndices = hiddenIndices.slice(0, numberToHide);
		}
	}

	function getDisplayWord(word: string, index: number) {
		let guessedWord = guessText.split(' ')[index] || '';
		return [...word]
			.map((letter, i) => {
				let guessedLetter = guessedWord[i] || '';
				if (guessedLetter.toLowerCase() === letter.toLowerCase()) {
					return `<b>${letter}</b>`;
				} else if (guessedLetter) {
					return `<span style='color: red;'>${guessedLetter}</span>`;
				} else if (!hiddenIndices.includes(index) || (i === 0 && showFirstLetterOnly))
					return letter;
				else {
					return `<span style='color: transparent; text-decoration: underline; text-decoration-color: #5a5a5a;'>${letter}</span>`;
				}
			})
			.join('');
	}
</script>

<div class="app-container">
	<div class="content-wrapper">
		<UserGuide />
		<div class="control-panel">
			<div class="switch-group">
				<Switch label="Voice Recognition" onclick={() => toggleSpeechRecognition()} />
				<Switch
					label="Show First Letters"
					onclick={() => (showFirstLetterOnly = !showFirstLetterOnly)}
				/>
			</div>

			<div class="slider-group">
				<label for="hide-slider" class="slider-label">
					Hide {numberToHide} words
				</label>
				<input
					type="range"
					id="hide-slider"
					min="0"
					max={numberOfWords}
					bind:value={numberToHide}
					oninput={updateHiddenWords}
					class="slider"
				/>
			</div>
		</div>

		<div class="verse-panel">
			{#if show}
				<p class="verse-text">{words.join(' ')}</p>
			{:else}
				<p class="verse-text">
					{@html words.map((word, index) => getDisplayWord(word, index)).join(' ')}
				</p>
			{/if}

			<div class="input-overlay">
				<textarea bind:value={guessText} spellcheck="false"></textarea>
			</div>
		</div>

		<div class="action-buttons">
			<button class="btn secondary" onclick={() => goto('/')}> Back to Home </button>
			<button class="btn primary" onclick={() => (show = !show)}>
				{show ? 'Hide Words' : 'Show All Words'}
			</button>
		</div>
	</div>
</div>

<style>
	.app-container {
		min-height: 100vh;
		background: linear-gradient(135deg, #f8fafc 0%, #e2e8f0 100%);
		padding: 2rem 1rem;
		margin-top: 4rem;
	}

	.content-wrapper {
		max-width: 1000px;
		margin: 0 auto;
		padding: 2rem;
	}

	.control-panel {
		background: white;
		border-radius: 16px;
		padding: 1.5rem;
		box-shadow: 0 4px 20px rgba(0, 0, 0, 0.06);
		margin-bottom: 2rem;
	}

	.switch-group {
		display: flex;
		justify-content: space-between;
		gap: 1rem;
		margin-bottom: 1.5rem;
	}

	.slider-group {
		display: flex;
		flex-direction: column;
		gap: 0.75rem;
	}

	.slider-label {
		color: #4a5568;
		font-size: 0.9rem;
		font-weight: 500;
	}

	.slider {
		width: 100%;
		height: 6px;
		background: #e2e8f0;
		border-radius: 3px;
		outline: none;
		accent-color: #2c5282;
	}

	.verse-panel {
		position: relative;
		background: white;
		border-radius: 16px;
		padding: 2rem;
		margin: 1.7rem 0;
		min-height: 300px;
		box-shadow: 0 4px 20px rgba(0, 0, 0, 0.06);
	}

	.verse-text {
		font-size: 1.25rem;
		line-height: 1.8;
		color: #4a5568;
		margin: 0;
		white-space: pre-wrap;
		word-spacing: 0.25rem;
	}

	.input-overlay {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		z-index: 9;
	}

	.input-overlay textarea {
		width: 100%;
		height: 100%;
		padding: 2rem;
		background: transparent;
		border: none;
		font-size: 1px;
		color: transparent;
		resize: none;
		border-radius: 16px;
	}

	.input-overlay textarea:focus {
		outline: 2px solid #2c5282;
	}

	.btn {
		padding: 0.75rem 1.5rem;
		margin: 0.3rem;
		border: none;
		border-radius: 8px;
		font-size: 1rem;
		font-weight: 500;
		cursor: pointer;
		transition: all 0.2s;
		box-shadow: #4a5568;
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

	.btn.secondary {
		background: #f1f5f9;
		color: #4a5568;
	}

	.btn.secondary:hover {
		background: #e2e8f0;
	}

	@media (max-width: 768px) {
		.content-wrapper {
			padding: 1rem;
		}

		.switch-group {
			flex-direction: column;
		}

		.verse-text {
			font-size: 1.1rem;
		}

		.action-buttons {
			flex-direction: column;
		}

		.btn {
			width: 100%;
		}
	}
</style>
