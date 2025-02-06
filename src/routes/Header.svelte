<script lang="ts">
	import { goto } from '$app/navigation';
</script>

<header>
	<div class="container">
		<h1 onclick={() => goto('/')}>MemorVerse</h1>
		<nav>
			<!-- <a href="/memorize">Memorize</a> -->
		</nav>
	</div>
</header>

<style>
	header {
		background-color: #2c5282; 
		color: white;
		width: 100%;
		padding: 1rem 2rem;
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

	nav {
		display: flex;
		gap: 1.5rem;
	}

	nav a {
		color: white;
		text-decoration: none;
		font-size: 1.1rem;
		font-weight: 500;
		transition: color 0.3s ease-in-out;
	}

	nav a:hover {
		color: #90cdf4; 
	}

	/* Responsive Design */
	@media (max-width: 768px) {
		.container {
			flex-direction: column;
			align-items: center;
			text-align: center;
			gap: 1rem;
		}

		nav {
			flex-direction: column;
			gap: 0.75rem;
		}
	}
</style>
