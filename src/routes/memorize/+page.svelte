<script lang="ts">
	import { goto } from '$app/navigation';
	import Header from '../Header.svelte';
	import { inputText } from '../state.svelte';

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
	let initialized = false

	$effect(() => {
		for (let i = 0; i<text.length; i++) {

			if (!/^[a-zA-Z]+$/.test(text[i])) {
				//this is to deal with punctuation when speaking. 
				// it automatically fills forward punctuation
				guessText = guessText.slice(0, i) + text[i] + guessText.slice(i+1)
				//marks whatever the last non-letter ther is to the last correct index
				lastCorrectIndex = i
			}
			//if it doesn't match make it so everytime i talk it rewrites over the last place
			if (guessText[i] !== text[i].toLowerCase()) {
				break
			}
		}
	})

	function initializeSpeechRecognition() {
		initialized = true
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
	};

	$effect(() => {
		if (recognition) {
			recognition.stop();
		}
	});

	function toggleSpeechRecognition() {
		if (!initialized) {initializeSpeechRecognition()}
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
					return `<span style='color: red; '>${guessedLetter}</span>`;
				} else if (!hiddenIndices.includes(index) || (i === 0 && showFirstLetterOnly))
					return letter;
				else {
					return `<span style='color: transparent; text-decoration: underline; text-decoration-color: #5a5a5a;'>${letter}</span>`;
				}
			})
			.join('');
	}

</script>

<div class="memory-page">
	<Header />

	<div class="verse-container">
		{#if show}
			<p>{words.join(' ')}</p>
		{:else}
			<p>
				{@html words.map((word, index) => getDisplayWord(word, index)).join(' ')}
			</p>
		{/if}
		<div class="input-container">
			<textarea bind:value={guessText} spellcheck="false"></textarea>
		</div>
		<div class="slider-container">
			<input
				type="range"
				id="hide-slider"
				min="0"
				max={numberOfWords}
				bind:value={numberToHide}
				oninput={updateHiddenWords}
				class="slider"
			/>
			<label for="hide-slider">Hide {numberToHide} words</label>
		</div>
	</div>

	<div class="button-container">
		<button onclick={() => goto('/')}>Back</button>
		<button onclick={() => (show = !show)}>
			{show ? 'Blank hidden words' : 'Show hidden words'}
		</button>
		<button onclick={() => (showFirstLetterOnly = !showFirstLetterOnly)}>
			{showFirstLetterOnly ? 'Hide all letters' : 'Show first letters'}
		</button>
		<button 
			onclick={toggleSpeechRecognition}
			class={isListening ? 'listening' : ''}
		>
			{isListening ? 'Stop Voice Input' : 'Start Voice Input'}
		</button>
	</div>
</div>

<style>
	.memory-page {
		padding-top: 7rem;
		min-height: 100vh;
		background: linear-gradient(to bottom right, #f8f9ff, #e6f0ff);
	}

	.verse-container {
		position: relative;
		max-width: 90%;
		margin: 2rem auto;
		padding: 2rem;
		background: white;
		border-radius: 10px;
		box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
	}

	.input-container {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		z-index: 9;
	}

	.input-container textarea {
		width: 99.99%;
		height: 99.99%;
		background: transparent;
		border: none;
		outline-color: #2c5282;
		text-align: start;
		font-size: 1 rem;
		color: transparent;
		border-radius: 10px;
	}

	.button-container {
		max-width: 800px;
		margin: 2rem auto;
		display: flex;
		flex-wrap: wrap;
		justify-content: center;
		gap: 1rem;
	}

	.slider-container {
		max-width: 800px;
		margin: 2rem auto;
		display: flex;
		flex-direction: column;
		align-items: center;
		color: #5a5a5a;
		position: relative;
		z-index: 10;
	}

	.slider {
		width: 80%;
		margin: 1rem 0;
		accent-color: #2c5282;
	}

	p {
		font-size: 1.4rem;
		line-height: 1.8;
		margin: 2rem 0;
		font-family: system-ui, -apple-system, sans-serif;
		white-space: pre-wrap;
		word-spacing: 0.5rem;
		color: #606060;
	}

	button {
		background-color: #2c5282;
		border: none;
		color: white;
		padding: 0.75rem 1.5rem;
		text-align: center;
		text-decoration: none;
		display: inline-block;
		font-size: 1rem;
		cursor: pointer;
		border-radius: 8px;
		transition: all 0.3s ease;
		box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
	}

	button:hover {
		background-color: #3182ce;
		transform: translateY(-1px);
	}

	button.listening {
		background-color: #e53e3e;
		animation: pulse 2s infinite;
	}

	@keyframes pulse {
		0% {
			box-shadow: 0 0 0 0 rgba(229, 62, 62, 0.4);
		}
		70% {
			box-shadow: 0 0 0 10px rgba(229, 62, 62, 0);
		}
		100% {
			box-shadow: 0 0 0 0 rgba(229, 62, 62, 0);
		}
	}
</style>