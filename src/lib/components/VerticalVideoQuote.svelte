<script lang="ts">
	import { base } from '$app/paths';

	export let src: string | undefined;
	export let quote: string = '';
	export let attribution: string | undefined;
	export let role: string | undefined;

	let videoEl: HTMLVideoElement;
	let paused = true;
	let muted = true;

	function togglePlay() {
		if (!videoEl) return;
		if (videoEl.paused) {
			videoEl.play();
			paused = false;
		} else {
			videoEl.pause();
			paused = true;
		}
	}

	function toggleMute() {
		if (!videoEl) return;
		videoEl.muted = !videoEl.muted;
		muted = videoEl.muted;
	}
</script>

{#if quote || src}
	<figure class="my-4 vertical-video-quote">
		<div class="container-fluid">
			<div class="row justify-content-start">
				<div class="col-12">
					<div class="vvq-layout">
						<!-- Quote: always visible — now on the LEFT -->
						<blockquote class="vvq-quote">
							<p class="vvq-quote-text">{quote}</p>
							{#if attribution || role}
								<footer class="vvq-attribution">
									{#if attribution}
										<cite class="vvq-name">{attribution}</cite>
									{/if}
									{#if role}
										<span class="vvq-role">{role}</span>
									{/if}
								</footer>
							{/if}
						</blockquote>

						{#if src}
							<!-- Video: hidden on mobile, shown md+ — now on the RIGHT -->
							<div class="vvq-video d-none d-md-flex">
								<div class="vvq-video-inner">
									<div class="vvq-frame" style="background-image: 'none'">
										<img
											src="{base}/photos/verticalframe.png"
											alt=""
											loading="lazy"
											style="height: 100%; width:100%"
										/>
										<video bind:this={videoEl} {src} muted playsinline preload="none" style="position: absolute;"></video>
										
										<!-- Custom controls -->
										<div class="vvq-controls">
											<button class="vvq-btn" on:click={togglePlay} aria-label={paused ? 'Play' : 'Pause'}>
												{#if paused}
													<svg viewBox="0 0 24 24" fill="currentColor" width="20" height="20">
														<path d="M8 5v14l11-7z" />
													</svg>
												{:else}
													<svg viewBox="0 0 24 24" fill="currentColor" width="20" height="20">
														<path d="M6 19h4V5H6v14zm8-14v14h4V5h-4z" />
													</svg>
												{/if}
											</button>

											<button class="vvq-btn" on:click={toggleMute} aria-label={muted ? 'Unmute' : 'Mute'}>
												{#if muted}
													<svg viewBox="0 0 24 24" fill="currentColor" width="20" height="20">
														<path d="M16.5 12A4.5 4.5 0 0 0 14 7.97v2.21l2.45 2.45c.03-.2.05-.41.05-.63zm2.5 0c0 .94-.2 1.82-.54 2.64l1.51 1.51A8.796 8.796 0 0 0 21 12c0-4.28-2.99-7.86-7-8.77v2.06c2.89.86 5 3.54 5 6.71zM4.27 3L3 4.27 7.73 9H3v6h4l5 5v-6.73l4.25 4.25c-.67.52-1.42.93-2.25 1.18v2.06A8.99 8.99 0 0 0 17.73 18L19 19.27 20.27 18 5.27 3 4.27 3zM12 4L9.91 6.09 12 8.18V4z" />
													</svg>
												{:else}
													<svg viewBox="0 0 24 24" fill="currentColor" width="20" height="20">
														<path d="M3 9v6h4l5 5V4L7 9H3zm13.5 3A4.5 4.5 0 0 0 14 7.97v8.05c1.48-.73 2.5-2.25 2.5-4.02zM14 3.23v2.06c2.89.86 5 3.54 5 6.71s-2.11 5.85-5 6.71v2.06c4.01-.91 7-4.49 7-8.77s-2.99-7.86-7-8.77z" />
													</svg>
												{/if}
											</button>
										</div>
									</div>
								</div>
							</div>
						{/if}
					</div>
				</div>
			</div>
		</div>
	</figure>
{/if}

<style>
	.vvq-layout {
		display: flex;
		flex-direction: row;
		align-items: stretch;
		gap: 2rem;
		min-height: 400px;
		justify-content: flex-start;
	}

	.vvq-video {
		flex: 0 0 auto;
		width: 280px;
		display: flex;
		align-items: center;
	}

	.vvq-video-inner {
		position: relative;
		width: 100%;
		padding-top: calc(100% * 16 / 9);
		border-radius: 0;
		background: transparent;
	}

	.vvq-frame {
		position: absolute;
		inset: 0;
		background-image: url('/photos/verticalframe.png');
		background-size: contain;
		background-repeat: no-repeat;
		background-position: center;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.vvq-frame video {
		position: relative;
		width: 84%;
		height: 85%;
		border: none;
	}

	/* Custom controls — positioned inside frame */
	.vvq-controls {
		position: absolute;
		bottom: 12%;
		left: 50%;
		transform: translateX(-50%);
		width: 80%;
		display: flex;
		justify-content: space-between;
		pointer-events: none;
		z-index: 10;
	}

	.vvq-btn {
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

	.vvq-btn:hover {
		background: rgba(0, 0, 0, 0.8);
	}

	.vvq-quote {
		flex: 3 1 0;
		display: flex;
		flex-direction: column;
		justify-content: center;
		margin: 0;
		padding: 1rem 1.5rem 1rem 0;
		border-right: 3px solid currentColor;
	}

	@media (max-width: 767.98px) {
		.vvq-quote {
			padding-right: 0;
			border-right: none;
		}
	}

	.vvq-quote-text {
		font-size: clamp(1.15rem, 2.5vw, 1.5rem);
		font-style: italic;
		line-height: 1.5;
		margin: 0 0 1rem;
	}

	.vvq-attribution {
		font-family: 'oswald', sans serif;
		display: flex;
		flex-direction: column;
		gap: 0.2rem;
	}

	.vvq-name {
		font-style: normal;
		font-weight: 600;
		font-size: 1.6rem;
	}

	.vvq-role {
		font-size: 1rem;
		opacity: 0.65;
	}
</style>