<script lang="ts">
	import { base } from '$app/paths';

	/**
	 * Example shortcode usage:
	 * [[TikTokHero
	 *   src1="videos/vid1.mp4" ... src30="videos/vid30.mp4"
	 *   headline="When birth control meets the algorithm"
	 *   deck="Your deck text here."
	 *   byline="By Jane Doe"
	 * ]]
	 *
	 * 10 columns, each with 3 stacked videos that loop seamlessly bottom to top.
	 */

	export let src1: string = '';
	export let src2: string = '';
	export let src3: string = '';
	export let src4: string = '';
	export let src5: string = '';
	export let src6: string = '';
	export let src7: string = '';
	export let src8: string = '';
	export let src9: string = '';
	export let src10: string = '';

	export let src11: string = '';
	export let src12: string = '';
	export let src13: string = '';
	export let src14: string = '';
	export let src15: string = '';
	export let src16: string = '';
	export let src17: string = '';
	export let src18: string = '';
	export let src19: string = '';
	export let src20: string = '';

	export let src21: string = '';
	export let src22: string = '';
	export let src23: string = '';
	export let src24: string = '';
	export let src25: string = '';
	export let src26: string = '';
	export let src27: string = '';
	export let src28: string = '';
	export let src29: string = '';
	export let src30: string = '';

	export let headline: string = '';
	export let deck: string = '';
	export let byline: string = '';

	let scrollY = 0;
	let heroElement: HTMLElement;

	$: allSrcs = [
		src1,
		src2,
		src3,
		src4,
		src5,
		src6,
		src7,
		src8,
		src9,
		src10,
		src11,
		src12,
		src13,
		src14,
		src15,
		src16,
		src17,
		src18,
		src19,
		src20,
		src21,
		src22,
		src23,
		src24,
		src25,
		src26,
		src27,
		src28,
		src29,
		src30
	];

	// Each column gets 3 videos: one from each group of 10
	$: columns = Array.from({ length: 10 }, (_, i) => [allSrcs[i], allSrcs[i + 10], allSrcs[i + 20]]);

	$: heroTop = heroElement ? heroElement.getBoundingClientRect().top : 0;
	$: isHeaderVisible =
		typeof window !== 'undefined' && heroElement
			? heroElement.getBoundingClientRect().top + 200 * window.innerHeight > scrollY
			: true; // No need for scroll tracking
	// =======================================================================

	const durations = [18, 22, 16, 24, 20, 17, 23, 19, 21, 15];
	const delays = [0, -6, -12, -4, -16, -9, -3, -14, -7, -11];
</script>

<svelte:window bind:scrollY />

<section class="tth-hero" bind:this={heroElement}>
	<div class="tth-grid">
		{#each columns as col, colIdx}
			<div class="tth-col-wrap">
				<div
					class="tth-col-strip"
					style="
            animation-duration: {durations[colIdx]}s;
            animation-delay: {delays[colIdx]}s;
          "
				>
					<!-- Videos stacked once -->
					{#each col as src}
						{#if src}
							<video src="{base}/{src}" autoplay muted loop playsinline disablepictureinpicture
							></video>
						{/if}
					{/each}
					<!-- Duplicate for seamless loop -->
					{#each col as src}
						{#if src}
							<video src="{base}/{src}" autoplay muted loop playsinline disablepictureinpicture
							></video>
						{/if}
					{/each}
				</div>
			</div>
		{/each}
	</div>
	<div id="headeroverlay"></div>

	<!-- Header box -->
	{#if headline || deck || byline}
		<div class="tth-header-box" class:hidden={!isHeaderVisible}>
			{#if headline}
				<h1 class="tth-headline">{headline}</h1>
			{/if}
			{#if deck}
				<p class="tth-deck">{deck}</p>
			{/if}
			{#if byline}
				<p class="tth-byline">{byline}</p>
			{/if}
		</div>
	{/if}
	<!-- ======================================================= -->
</section>

<style>
	.tth-hero {
		position: relative;
		width: 100vw;
		height: 200vh;
		background: #151e3f;
		left: 50%;
		right: 50%;
		margin-left: -50vw;
		margin-right: -50vw;
	}

	.tth-grid {
		position: absolute;
		inset: 0;
		display: flex;
		flex-direction: row;
		gap: 6px;
		width: 100%;
		height: 100%;
	}

	.tth-col-wrap {
		flex: 1 1 0;
		overflow: hidden;
		position: relative;
	}

	.tth-col-strip {
		display: flex;
		flex-direction: column;
		animation: riseUp linear infinite;
		will-change: transform;
		gap: 6px; /* ← Add this for vertical spacing */
	}

	.tth-col-strip video {
		width: 100%;
		flex-shrink: 0;
		display: block;
		object-fit: cover;
		height: calc(100vh / 3);
	}

	@keyframes riseUp {
		0% {
			transform: translateY(0);
		}
		100% {
			transform: translateY(-50%);
		}
	}

	.tth-header-box {
		position: sticky;
		top: 25%;
		left: 50%;
		transform: translate(-50%, -50%);
		width: 45%;
		height: fit-content;
		background: #533b4d;
		padding: 2rem 2.25rem;
		border: 2px solid #fffff3;
		z-index: 10;
		transition: opacity 0.3s ease;
	}

	.tth-header-box.hidden {
		display: none;
	}

	.tth-headline {
		color: #fffff3;
		font-size: clamp(1.7rem, 2.5vw, 2.75rem);
		font-weight: 700;
		line-height: 1.15;
		margin: 0 0 1rem;
	}

	.tth-deck {
		color: #fffff3;
		font-size: clamp(1.3rem, 1.2vw, 1.1rem);
		line-height: 1.6;
		margin: 0 0 1.25rem;
	}

	.tth-byline {
		color: #fffff3;
		font-family: 'Oswald', sans-serif;
		font-size: 1rem;
		text-transform: uppercase;
		letter-spacing: 0.08em;
		margin: 0;
		opacity: 0.7;
	}

	#headeroverlay {
		height: 100%;
		width: 100%;
		position: absolute;
		top: 0;
		left: 0;
		background: #151e3f;
		opacity: 0.5;
	}
</style>
