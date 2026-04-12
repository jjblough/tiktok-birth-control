<script lang="ts">
  /**
   * Example shortcode usage:
   * [[TikTokHero
   *   src1="videos/vid1.mp4" ... src30="videos/vid30.mp4"
   *   headline="Your headline here"
   *   deck="A longer description of the story goes here."
   *   byline="By Jane Doe"
   * ]]
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

  const delays    = [0, -3, -6, -1.5, -8, -4.5, -2, -7, -5, -9];
  const durations = [18, 22, 16, 24, 20, 17, 23, 19, 21, 15];

  $: rows = [
    [src1,  src2,  src3,  src4,  src5,  src6,  src7,  src8,  src9,  src10],
    [src11, src12, src13, src14, src15, src16, src17, src18, src19, src20],
    [src21, src22, src23, src24, src25, src26, src27, src28, src29, src30],
  ];
</script>

<section class="tth-hero">

  <!-- Video grid background -->
  <div class="tth-grid">
    {#each rows as row, rowIdx}
      <div class="tth-row">
        {#each row as src, colIdx}
          {#if src}
            <div
              class="tth-col"
              style="
                animation-duration: {durations[colIdx]}s;
                animation-delay: {delays[colIdx]}s;
              "
            >
              <video
                src="/{src}"
                autoplay
                muted
                loop
                playsinline
                disablepictureinpicture
              ></video>
            </div>
          {/if}
        {/each}
      </div>
    {/each}
  </div>

  <!-- Dark overlay -->
  <div class="tth-overlay"></div>

  <!-- Header box: left third -->
  <div class="tth-header-box">
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

</section>

<style>
  .tth-hero {
    position: relative;
    width: 100vw;
    height: 100vh;
    overflow: hidden;
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
    flex-direction: column;
    width: 100%;
    height: 100%;
    gap: 4px;
  }

  .tth-row {
    display: flex;
    flex-direction: row;
    flex: 1 1 0;
    gap: 4px;
    overflow: hidden;
  }

  .tth-col {
    flex: 1 1 0;
    overflow: hidden;
    animation: riseUp linear infinite;
  }

  .tth-col video {
    width: 100%;
    height: 100%;
    display: block;
    object-fit: cover;
  }

  @keyframes riseUp {
    0%   { transform: translateY(60%); }
    100% { transform: translateY(-60%); }
  }

  /* Dark overlay behind the text box */
  .tth-overlay {
    position: absolute;
    inset: 0;
    pointer-events: none;
  }

  /* Header box: sits in left third, vertically centered */
  .tth-header-box {
    position: absolute;
    top: 50%;
    left: 20%;
    transform: translateY(-50%);
    width: 45%; /* roughly left third */
    background: #4c86a8;
    backdrop-filter: none;
    -webkit-backdrop-filter: none;
    padding: 2rem 2.25rem;
    border-left: none;
     border: 2px solid #fff;
  }

  .tth-headline {
    color: #fff;
    font-size: clamp(1.5rem, 2.5vw, 2.75rem);
    font-weight: 700;
    line-height: 1.15;
    margin: 0 0 1rem;
  }

  .tth-deck {
    color: rgba(255, 255, 255, 0.85);
    font-size: clamp(0.9rem, 1.2vw, 1.1rem);
    line-height: 1.6;
    margin: 0 0 1.25rem;
  }

  .tth-byline {
    color: rgba(255, 255, 255, 0.6);
    font-size: 0.8rem;
    text-transform: uppercase;
    letter-spacing: 0.08em;
    margin: 0;
  }
</style>