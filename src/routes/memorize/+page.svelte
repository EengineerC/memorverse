<script lang="ts">
	import { goto } from '$app/navigation';
	import Header from '../Header.svelte';
	import { inputText } from '../state.svelte';

	let numberToHide = $state(0);
	let hiddenIndices: number[] = $state([]);
	let show = $state(false);
	let showFirstLetterOnly = $state(false);
	let guessText = $state('');

	let text = inputText.text;
	const words = text.split(' ');

	const numberOfWords = words.length;

	$inspect('guess', guessText);
	$inspect('words', words);

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

	// I know this function is a little hard to read but it fits the goal perfectly
	function getDisplayWord(word: string, index: number) {
		let guessedWord = guessText.split(' ')[index] || '';
		return [...word]
			.map((letter, i) => {
				let guessedLetter = guessedWord[i] || '';
				if (guessedLetter.toLowerCase() === letter.toLowerCase()) {
					return `<b>${letter}</b>`; //bolds if guess letter matches the verse
				} else if (guessedLetter) {
					return `<span style='color: red; '>${guessedLetter}</span>`; //makes red if guess letter doesnt match verse
				} else if (!hiddenIndices.includes(index) || (i === 0 && showFirstLetterOnly))
					return letter; 
				else { //blanks if non of those are true
					return `<span style='color: transparent; text-decoration: underline; text-decoration-color: #5a5a5a;'>${letter}</span>`;
				}
			}
		).join('');
	}

	//to keep people from accidently typing at the beginning randomly
	function moveCursorToEnd(event: Event) {
		const input = event.target as HTMLInputElement;

		input.selectionStart = input.selectionEnd = input.value.length;
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
			<input bind:value={guessText} spellcheck="false" oninput={moveCursorToEnd}/>
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
		<button onclick={() => (show = !show)}
			>{show ? 'Blank hidden words' : 'Show hidden words'}</button
		>
		<button onclick={() => (showFirstLetterOnly = !showFirstLetterOnly)}>
			{showFirstLetterOnly ? 'Hide all letters' : 'Show first letters'}
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

	.input-container input {
		width: 99.99%;
		height: 99.99%;
		background: transparent;
		border: none;
		outline-color: #2c5282;
		text-align: top;
		font-size: 1.4rem;
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
		font-family:
			system-ui,
			-apple-system,
			sans-serif;
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
</style>
