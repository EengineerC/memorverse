<script lang="ts">
	export let label: string;
	export let fontSize: number = 16;
	export let value: string = 'on';
	export let onclick: ((checked: boolean) => void) | undefined = undefined;

	let checked: boolean = false;
	const uniqueID: number = Math.floor(Math.random() * 100);

	function handleClick(): void {
		checked = !checked;
		value = checked ? 'on' : 'off';

		if (onclick) {
			onclick(checked);
		}
	}
</script>

<div class="s s--slider" style="font-size:{fontSize}px">
	<span id={`switch-${uniqueID}`}>{label}</span>
	<button
		role="switch"
		aria-checked={checked}
		aria-labelledby={`switch-${uniqueID}`}
		onclick={handleClick}
	>
	</button>
</div>

<style>
	:root {
		--accent-color: #2c5282;
		--gray: #ccc;
	}

	.s--slider {
		display: flex;
		align-items: center;
		padding: px;
	}

	.s--slider button {
		width: 3em;
		height: 1.6em;
		position: relative;
		margin: 0 0 0 0.5em;
		background: var(--gray);
		border: none;
		border-radius: 1.5em;
	}

	.s--slider button::before {
		content: '';
		position: absolute;
		width: 1.3em;
		height: 1.3em;
		background: #fff;
		top: 0.13em;
		right: 1.5em;
		border-radius: 100%;
		transition: transform 0.3s;
	}

	.s--slider button[aria-checked='true'] {
		background-color: var(--accent-color);
	}

	.s--slider button[aria-checked='true']::before {
		transform: translateX(1.3em);
	}

	.s--slider button:focus {
		box-shadow: 0 0px 8px var(--accent-color);
	}
</style>
