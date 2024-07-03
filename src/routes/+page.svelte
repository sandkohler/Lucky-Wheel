<script lang="ts">
	import { onMount } from "svelte";

	type WheelValue = {
		label: string;
		color: string;
	};

	const values: WheelValue[] = [
		{ label: "Preis 1", color: "#FFC0CB" },
		{ label: "Preis 2", color: "#FFD700" },
		{ label: "Preis 3", color: "#FFA500" },
		{ label: "Preis 4", color: "#8A2BE2" },
		{ label: "Preis 5", color: "#00FF00" },
	];

	let canvas: HTMLCanvasElement;
	let ctx: CanvasRenderingContext2D;
	let anglePerSegment: number;
	let currentRotation = 0;

	onMount(() => {
		ctx = canvas.getContext("2d");
		drawWheel();
	});

	function drawWheel(): void {
		const radius = canvas.width / 2;
		ctx.clearRect(0, 0, canvas.width, canvas.height);
		anglePerSegment = (2 * Math.PI) / values.length;

		values.forEach((value, index) => {
			ctx.beginPath();
			ctx.moveTo(radius, radius);
			ctx.arc(
				radius,
				radius,
				radius,
				anglePerSegment * index,
				anglePerSegment * (index + 1),
			);
			ctx.fillStyle = value.color;
			ctx.fill();
			ctx.save();
			ctx.translate(radius, radius);
			ctx.rotate(anglePerSegment * (index + 0.5));
			ctx.textAlign = "right";
			ctx.fillStyle = "#000";
			ctx.fillText(value.label, radius - 10, 0);
			ctx.restore();
		});
	}

	function spinWheel(): void {
		const spins = Math.floor(Math.random() * 10 + 5); // 5-15 Drehungen
		const targetRotation =
			currentRotation + spins * 360 + Math.random() * 360;
		const animation = canvas.animate(
			[
				{ transform: `rotate(${currentRotation}deg)` },
				{ transform: `rotate(${targetRotation}deg)` },
			],
			{
				duration: 3000,
				easing: "ease-out",
			},
		);
		animation.onfinish = () => {
			currentRotation = targetRotation % 360;
			// Logik
		};
	}
</script>

<canvas bind:this={canvas} width="500" height="500"></canvas>
<button on:click={spinWheel}>Dreh das Rad!</button>

<style>
	canvas {
		border: 2px solid black;
		border-radius: 50%;
	}
</style>
