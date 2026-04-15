<script lang="ts">

  /**
   * Example shortcode usage:
   * [[VerticalVideoQuote src="https://example.com/embed" quote="The world changed overnight." attribution="Jane Doe" role="Community Organizer"]]
   */

  export let src: string | undefined;
  export let quote: string = '';
  export let attribution: string | undefined;
  export let role: string | undefined;
  export let title: string = '';
</script>

{#if quote || src}
  <figure class="my-4 vertical-video-quote">
    <div class="container-fluid">
      <div class="row justify-content-start">
  <div class="col-12">
          <div class="vvq-layout">

            {#if src}
  <!-- Video: hidden on mobile, shown md+ -->
  <div class="vvq-video d-none d-md-flex">
    <div class="vvq-video-inner">
      <div class="vvq-frame">
        <iframe {src} {title} allowfullscreen></iframe>
      </div>
    </div>
  </div>
{/if}

            <!-- Quote: always visible -->
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

.vvq-frame iframe {
  position: relative;
  width: 90%;   /* adjust to fit inside your frame image */
  height: 90%;  /* adjust to fit inside your frame image */
  border: none;
}
  /* Quote column: fills remaining space */
  .vvq-quote {
    flex: 3 1 0;
    display: flex;
    flex-direction: column;
    justify-content: center;
    margin: 0;
    padding: 1rem 0 1rem 1.5rem;
    border-left: 3px solid currentColor;
  }

  /* On mobile there's no video, so remove the left border indent feel */
  @media (max-width: 767.98px) {
    .vvq-quote {
      padding-left: 0;
      border-left: none;
    }
  }

  .vvq-quote-text {
    font-size: clamp(1.15rem, 2.5vw, 1.5rem);
    font-style: italic;
    line-height: 1.5;
    margin: 0 0 1rem;
  }

  .vvq-attribution {
    display: flex;
    flex-direction: column;
    gap: 0.2rem;
  }

  .vvq-name {
    font-style: normal;
    font-weight: 600;
    font-size: 0.95rem;
  }

  .vvq-role {
    font-size: 0.85rem;
    opacity: 0.65;
  }
</style>