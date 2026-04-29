<script lang="ts">
	/**
	 * Example shortcode usage:
	 * [[VerticalVideoQuote src="https://example.com/embed" quote="The world changed overnight." attribution="Jane Doe" role="Community Organizer"]]
	 */

	import { base } from '$app/paths';

	export let src: string | undefined;
	export let quote: string = '';
	export let attribution: string | undefined;
	export let role: string | undefined;
</script>

{#if quote || src}
	<figure class="my-4 vertical-video-quote">
		<div class="container-fluid">
			<div class="row justify-content-start">
				<div class="col-12">
					<div class="vvq-layout">
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
											<video {src} muted controls playsinline style="position: absolute;"></video>
										</div>
									</div>
								</div>
							{/if}
						</div>
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

	/* Video column: fixed width to match a 9:16 aspect ratio at a reasonable height */
	.vvq-video {
		flex: 0 0 auto;
		width: 280px;
		display: flex;
		align-items: center;
	}

	.vvq-video-inner {
		position: relative;
		width: 100%;
		/* 16:9 flipped = 9:16 */
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
	/* Quote column: fills remaining space */
	.vvq-quote {
		flex: 3 1 0;
		display: flex;
		flex-direction: column;
		justify-content: center;
		margin: 0;
		padding: 1rem 1.5rem 1rem 0; /* was padding-left, now padding-right */
		border-right: 3px solid currentColor; /* was border-left */
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
		font-family:
			'oswald',
			sans serif;
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
