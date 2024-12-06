<script>
	import { onMount } from 'svelte';
	import { page } from '$app/stores';

	let canvas;
	let ctx;
	const snowflakes = [];
	let name;

	$: name = $page.params.name;
	$: message = $page.params.message;

	function createSnowflakes() {
		for (let i = 0; i < 200; i++) {
			snowflakes.push({
				x: Math.random() * canvas.width,
				y: Math.random() * canvas.height,
				radius: Math.random() * 3 + 1,
				speed: Math.random() * 2 + 0.5
			});
		}
	}

	function drawSnowflakes() {
		ctx.clearRect(0, 0, canvas.width, canvas.height);

		snowflakes.forEach((flake) => {
			const gradient = ctx.createRadialGradient(
				flake.x,
				flake.y,
				0,
				flake.x,
				flake.y,
				flake.radius
			);
			gradient.addColorStop(0, 'rgba(255, 255, 255, 1)');
			gradient.addColorStop(0.5, 'rgba(255, 255, 255, 0.8)');
			gradient.addColorStop(1, 'rgba(255, 255, 255, 0)');

			ctx.fillStyle = gradient;
			ctx.beginPath();
			ctx.arc(flake.x, flake.y, flake.radius, 0, Math.PI * 2);
			ctx.fill();
		});
	}

	function updateSnowflakes() {
		snowflakes.forEach((flake) => {
			flake.y += flake.speed;
			if (flake.y > canvas.height) {
				flake.y = 0;
				flake.x = Math.random() * canvas.width;
			}
		});
	}

	function animate() {
		drawSnowflakes();
		updateSnowflakes();
		requestAnimationFrame(animate);
	}

	onMount(() => {
		canvas.width = window.innerWidth;
		canvas.height = window.innerHeight;
		ctx = canvas.getContext('2d');

		createSnowflakes();
		animate();

		// Adjust canvas size on resize
		const resizeHandler = () => {
			canvas.width = window.innerWidth;
			canvas.height = window.innerHeight;
		};
		window.addEventListener('resize', resizeHandler);

		// Cleanup
		return () => window.removeEventListener('resize', resizeHandler);
	});
</script>

<main>
	<div class="container">
		<div class="shooting-star"></div>
		<h1 class="wish crafty-girls-regular">Merry Christmas 2024</h1>
		<br />

		{#if message != 'false'}
			<h1 class="crafty-girls-regular" style="text-decoration:underline;">Message:)</h1>
			<q style="font-size: 2rem;color:rebeccapurple;" class="crafty-girls-regular">
				{message}
			</q>
		{/if}

		<h2 class="wish crafty-girls-regular" style="margin-top:6rem;">
			From: {name}
		</h2>
		<img
			class="tree"
			src="https://raw.githubusercontent.com/farazc60/Project-Images/refs/heads/main/christmas/christmasTree.webp"
			alt="Christmas Tree"
		/>
		<img
			class="snow-hill"
			src="https://raw.githubusercontent.com/farazc60/Project-Images/refs/heads/main/christmas/snowyHills.webp"
			alt="Snowy Hills"
		/>
	</div>
	<div id="gears">
		<!-- svelte-ignore a11y_consider_explicit_label -->
		<a href="../../">
			<span style="font-size: 1.8rem;">&nbsp;</span><i class="fa-solid fa-gears fa-2xl"></i>
		</a>
	</div>
	<canvas bind:this={canvas}></canvas>
</main>

<style>
	#gears {
		position: absolute;
		bottom: 50%;
	}

	/*Google Font*/
	.crafty-girls-regular {
		font-family: 'Crafty Girls', serif;
		font-weight: 400;
		font-style: normal;
	}

	.container {
		display: flex;
		justify-content: center;
		align-items: center;
		flex-direction: column;
	}

	.shooting-star {
		position: absolute;
		top: 0;
		left: -120px;
		width: 120px;
		height: 2px;
		background: url(https://raw.githubusercontent.com/farazc60/Project-Images/refs/heads/main/christmas/shooting-star.png)
			center center no-repeat;
		background-size: 100% 100%;
		transform: rotate(20deg);
		animation: falling-star 10s linear 0.2s infinite;
		z-index: 5;
	}

	@keyframes falling-star {
		0% {
			transform: translate3d(0, 0, 0) rotate(10deg);
			opacity: 1;
		}
		25%,
		100% {
			transform: translate3d(100vw, 30vh, 0) rotate(10deg);
			opacity: 0;
		}
	}

	.tree {
		position: absolute;
		bottom: 70px;
		width: 50%;
		max-width: 400px;
		height: auto;
		z-index: 1;
	}
	@media (max-width: 769px) {
		.tree {
			bottom: 10%;
		}
	}

	@keyframes fall {
		0% {
			transform: translate(-50%, -50%) rotate(0deg);
			opacity: 1;
		}
		50% {
			transform: translate(calc(-50% + 300px), calc(-50% + 500px)) rotate(180deg);
			opacity: 0.5;
		}
		100% {
			transform: translate(calc(-50% + 500px), calc(-50% + 700px)) rotate(360deg);
			opacity: 0;
		}
	}

	.snow-hill {
		position: absolute;
		width: 100%;
		bottom: 0;
	}

	.wish {
		margin: 0;
		text-align: center;
		font-size: 2rem;
		color: #fff;
		text-shadow:
			0 0 5px #ff0000,
			0 0 10px #ff3333,
			0 0 20px #ff6666;
		animation: sparkle 2s infinite;
		z-index: 8;
	}

	@media screen and (min-width: 769px) {
		.wish {
			font-size: 3rem;
		}
	}

	@keyframes sparkle {
		0%,
		100% {
			text-shadow:
				0 0 5px #ff0000,
				0 0 10px #ff3333,
				0 0 20px #ff6666,
				0 0 30px teal;
			transform: scale(1);
		}
		50% {
			text-shadow:
				0 0 10px #fcca05,
				0 0 20px #ff9900,
				0 0 30px #ff6600;
			transform: scale(1.1);
		}
	}

	canvas {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		z-index: 3;
		pointer-events: none;
	}
</style>
