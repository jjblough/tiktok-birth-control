<script lang="ts">
	/**
	 * Example shortcode usage:
	 * [[VideoTimeline
	 *   src="videos/myvideo.mp4"
	 *   description="In this 33-second video, you are looking at 46 fast-cut clips."
	 *   t1="0" label1="Preparing the chicken" detail1="6 clips"
	 *   t2="4" label2="Marinating the chicken" detail2="3 clips"
	 *   ...
	 * ]]
	 * Supports up to 10 chapters (t1–t10, label1–label10, detail1–detail10)
	 */

	import { base } from '$app/paths';
	import { onMount } from 'svelte';

	export let src: string = '';
	export let description: string = '';

	export let t1: string = '';  export let label1: string = '';  export let detail1: string = '';
	export let t2: string = '';  export let label2: string = '';  export let detail2: string = '';
	export let t3: string = '';  export let label3: string = '';  export let detail3: string = '';
	export let t4: string = '';  export let label4: string = '';  export let detail4: string = '';
	export let t5: string = '';  export let label5: string = '';  export let detail5: string = '';
	export let t6: string = '';  export let label6: string = '';  export let detail6: string = '';
	export let t7: string = '';  export let label7: string = '';  export let detail7: string = '';
	export let t8: string = '';  export let label8: string = '';  export let detail8: string = '';
	export let t9: string = '';  export let label9: string = '';  export let detail9: string = '';
	export let t10: string = ''; export let label10: string = ''; export let detail10: string = '';

	type Chapter = { time: number; label: string; detail: string };

	$: chapters = (
		[
			{ time: t1,  label: label1,  detail: detail1  },
			{ time: t2,  label: label2,  detail: detail2  },
			{ time: t3,  label: label3,  detail: detail3  },
			{ time: t4,  label: label4,  detail: detail4  },
			{ time: t5,  label: label5,  detail: detail5  },
			{ time: t6,  label: label6,  detail: detail6  },
			{ time: t7,  label: label7,  detail: detail7  },
			{ time: t8,  label: label8,  detail: detail8  },
			{ time: t9,  label: label9,  detail: detail9  },
			{ time: t10, label: label10, detail: detail10 },
		] as { time: string; label: string; detail: string }[]
	)
		.filter((c) => c.label.trim().length > 0)
		.map((c) => ({ time: parseFloat(c.time) || 0, label: c.label, detail: c.detail })) as Chapter[];

	let videoEl: HTMLVideoElement;
	let currentTime = 0;
	let paused = true;
	let muted = true;

	function tick() {
		if (typeof window === 'undefined') return;
		if (videoEl) currentTime = videoEl.currentTime;
		requestAnimationFrame(tick);
	}

	onMount(() => {
		tick();
	});

	function togglePlay() {
		if (!videoEl) return;
		if (videoEl.paused) { videoEl.play(); paused = false; }
		else                 { videoEl.pause(); paused = true; }
	}

	function toggleMute() {
		if (!videoEl) return;
		videoEl.muted = !videoEl.muted;
		muted = videoEl.muted;
	}

	function seekTo(time: number) {
		if (!videoEl) return;
		videoEl.currentTime = time;
		videoEl.play();
		paused = false;
	}

	function formatTime(s: number): string {
		const m = Math.floor(s / 60);
		const sec = Math.floor(s % 60);
		return `${m}:${sec.toString().padStart(2, '0')}`;
	}

	$: activeIdx = chapters.reduce((acc, ch, i) => {
		return currentTime >= ch.time ? i : acc;
	}, -1);
</script>

<div class="vt-wrap">
	<!-- Left: phone + video -->
	<div class="vt-phone-col">
		<div class="vt-phone">
			<div class="vt-frame-bg">
				<img src="{base}/photos/verticalframe.png" alt="" loading="lazy" class="vt-frame-img" aria-hidden="true" />
				<div class="vt-video-area">
					{#if src}
						<video bind:this={videoEl} src="{base}/{src}" muted playsinline loop preload="metadata" class="vt-video"></video>
					{/if}
				</div>
			</div>

			<!-- Controls overlay -->
			<div class="vt-controls">
				<button class="vt-btn" on:click={togglePlay} aria-label={paused ? 'Play' : 'Pause'}>
					{#if paused}
						<svg viewBox="0 0 24 24" fill="currentColor" width="20" height="20"><path d="M8 5v14l11-7z"/></svg>
					{:else}
						<svg viewBox="0 0 24 24" fill="currentColor" width="20" height="20"><path d="M6 19h4V5H6v14zm8-14v14h4V5h-4z"/></svg>
					{/if}
				</button>

				<button class="vt-btn" on:click={toggleMute} aria-label={muted ? 'Unmute' : 'Mute'}>
					{#if muted}
						<svg viewBox="0 0 24 24" fill="currentColor" width="20" height="20"><path d="M16.5 12A4.5 4.5 0 0 0 14 7.97v2.21l2.45 2.45c.03-.2.05-.41.05-.63zm2.5 0c0 .94-.2 1.82-.54 2.64l1.51 1.51A8.796 8.796 0 0 0 21 12c0-4.28-2.99-7.86-7-8.77v2.06c2.89.86 5 3.54 5 6.71zM4.27 3L3 4.27 7.73 9H3v6h4l5 5v-6.73l4.25 4.25c-.67.52-1.42.93-2.25 1.18v2.06A8.99 8.99 0 0 0 17.73 18L19 19.27 20.27 18 5.27 3 4.27 3zM12 4L9.91 6.09 12 8.18V4z"/></svg>
					{:else}
						<svg viewBox="0 0 24 24" fill="currentColor" width="20" height="20"><path d="M3 9v6h4l5 5V4L7 9H3zm13.5 3A4.5 4.5 0 0 0 14 7.97v8.05c1.48-.73 2.5-2.25 2.5-4.02zM14 3.23v2.06c2.89.86 5 3.54 5 6.71s-2.11 5.85-5 6.71v2.06c4.01-.91 7-4.49 7-8.77s-2.99-7.86-7-8.77z"/></svg>
					{/if}
				</button>
			</div>
		</div>
	</div>

	<!-- Right: description + timeline -->
	<div class="vt-right-col">
		{#if description}
			<p class="vt-description">{description}</p>
		{/if}

		{#if chapters.length > 0}
			<div class="vt-timeline">
				{#each chapters as ch, i (i)}
					<button
						class="vt-chapter"
						class:vt-chapter--active={i === activeIdx}
						class:vt-chapter--past={i < activeIdx}
						class:vt-chapter--last={i === chapters.length - 1}
						on:click={() => seekTo(ch.time)}
					>
						<div class="vt-dot-col">
							<div class="vt-dot"></div>
							<!-- Connecting line drawn per-chapter so it lives in scrollable flow -->
							{#if i < chapters.length - 1}
								<div class="vt-connector"></div>
							{/if}
						</div>
						<div class="vt-chapter-body">
							<span class="vt-time">{formatTime(ch.time)}</span>
							<span class="vt-label">{ch.label}</span>
							{#if ch.detail}
								<span class="vt-detail">({ch.detail})</span>
							{/if}
						</div>
					</button>
				{/each}
			</div>
		{/if}
	</div>
</div>

<style>
	.vt-wrap {
		font-family: 'Oswald', sans-serif;
		display: flex;
		flex-direction: row;
		align-items: flex-start;
		gap: 4rem;
		padding: 2rem 0;
		width: min(85vw, 1100px);
		margin-left: 50%;
		transform: translateX(-50%);
	}

	/* ── Phone column ── */
	.vt-phone-col {
		flex: 0 0 auto;
		width: 340px;
	}

	.vt-phone {
		position: relative;
		width: 340px;
	}

	.vt-frame-bg {
		position: relative;
		width: 100%;
	}

	.vt-frame-img {
		width: 100%;
		display: block;
		pointer-events: none;
		user-select: none;
	}

	.vt-video-area {
		position: absolute;
		top: 4.5%;
		left: 8%;
		width: 84%;
		height: 91%;
		overflow: hidden;
	}

	.vt-video {
		width: 100%;
		height: 100%;
		object-fit: cover;
		display: block;
	}

	.vt-controls {
		position: absolute;
		bottom: 8%;
		left: 0;
		width: 100%;
		display: flex;
		justify-content: space-between;
		padding: 0 12%;
		pointer-events: none;
	}

	.vt-btn {
		pointer-events: all;
		background: rgba(0, 0, 0, 0.55);
		border: none;
		border-radius: 50%;
		width: 36px;
		height: 36px;
		display: flex;
		align-items: center;
		justify-content: center;
		color: #fff;
		cursor: pointer;
		transition: background 0.15s;
	}

	.vt-btn:hover {
		background: rgba(0, 0, 0, 0.8);
	}

	/* ── Right column ── */
	.vt-right-col {
		flex: 1 1 0;
		display: flex;
		flex-direction: column;
		gap: 1.5rem;
		padding-top: 0.5rem;
		max-height: 70vh;
		overflow: hidden;
	}

	.vt-description {
		font-size: clamp(1rem, 1.5vw, 1.25rem);
		line-height: 1.5;
		margin: 0;
		color: #fffff3;
		flex-shrink: 0;
	}

	/* ── Timeline ── */
	.vt-timeline {
		display: flex;
		flex-direction: column;
		flex: 1;
		overflow-y: auto;
		overflow-x: hidden;
		padding-right: 1rem;
	}

	/* Each chapter is a row: [dot + connector] | [text] */
	.vt-chapter {
		display: flex;
		flex-direction: row;
		align-items: stretch; /* so dot-col fills the row height */
		gap: 1rem;
		padding: 0;
		background: none;
		border: none;
		cursor: pointer;
		text-align: left;
	}

	/* ── Dot column: dot on top, connector line below ── */
	.vt-dot-col {
		flex: 0 0 auto;
		width: 14px;
		display: flex;
		flex-direction: column;
		align-items: center;
		padding-top: 0.6rem; /* aligns dot with text baseline */
	}

	.vt-dot {
		flex: 0 0 auto;
		width: 12px;
		height: 12px;
		border-radius: 50%;
		background: rgba(128, 128, 128, 0.4);
		transition: background 0.2s, transform 0.2s;
	}

	/* Connector grows to fill whatever height the chapter text needs */
	.vt-connector {
		flex: 1 1 auto;
		width: 2px;
		min-height: 0.6rem; /* at least some gap */
		background: rgba(128, 128, 128, 0.3);
		margin-top: 4px;
	}

	.vt-chapter-body {
		display: flex;
		flex-direction: row;
		align-items: baseline;
		gap: 0.5rem;
		flex-wrap: wrap;
		padding: 0.6rem 0;
	}

	.vt-time {
		font-size: 0.85rem;
		font-variant-numeric: tabular-nums;
		color: #fffff3;
		opacity: 0.5;
		min-width: 2.5rem;
		transition: opacity 0.2s;
	}

	.vt-label {
		font-size: 1.05rem;
		font-weight: 500;
		opacity: 0.4;
		color: #fffff3;
		transition: opacity 0.2s, color 0.2s;
	}

	.vt-detail {
		font-size: 0.8rem;
		opacity: 0.35;
		color: #fffff3;
		transition: opacity 0.2s, color 0.2s;
	}

	/* Past */
	.vt-chapter--past .vt-dot       { background: #c08497; }
	.vt-chapter--past .vt-connector { background: #c08497; opacity: 0.4; }
	.vt-chapter--past .vt-time      { opacity: 0.6; }
	.vt-chapter--past .vt-label     { opacity: 0.7; }
	.vt-chapter--past .vt-detail    { opacity: 0.5; color: #c08497; }

	/* Active */
	.vt-chapter--active .vt-dot    { background: #c08497; transform: scale(1.3); }
	.vt-chapter--active .vt-time   { opacity: 1; }
	.vt-chapter--active .vt-label  { opacity: 1; }
	.vt-chapter--active .vt-detail { opacity: 1; color: #c08497; }

	/* Hover */
	.vt-chapter:hover .vt-label { opacity: 0.9; }
	.vt-chapter:hover .vt-dot   { transform: scale(1.2); }

	/* Mobile */
	@media (max-width: 767px) {
		.vt-wrap {
			flex-direction: column;
			gap: 2rem;
		}
		.vt-phone-col {
			width: 100%;
			max-width: 280px;
			margin: 0 auto;
		}
		.vt-right-col {
			max-height: none;
			overflow: visible;
			width: 100%;
		}
		.vt-timeline {
			overflow-y: visible;
			overflow-x: visible;
			padding-right: 0;
		}
	}
</style>