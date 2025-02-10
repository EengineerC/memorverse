<script lang="ts">
	import { goto } from '$app/navigation';
</script>

<header>
	<div class="container">
		<h1 onclick={() => goto('/')}>MemorVerse</h1>
		<!-- If I add more fetures I'll add a nav but it doesnt make sense right now -->
	</div>
</header>

<style>
	header {
		background-color: #2c5282; 
		color: white;
		width: 100%;
		padding: 2rem 3rem;
		box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
		position: fixed;
		top: 0;
		left: 0;
		z-index: 1000;
	}

	.container {
		display: flex;
		justify-content: space-between;
		align-items: center;
		max-width: 1200px;
		margin: 0 auto;
	}

	h1 {
		font-size: 2rem;
		font-weight: 700;
		cursor: pointer;
		transition: opacity 0.3s ease-in-out;
	}

	h1:hover {
		opacity: 0.8;
	}

	@media (max-width: 768px) {
		.container {
			flex-direction: column;
			align-items: center;
			text-align: center;
			gap: 1rem;
		}
	}
</style>
