<script lang="ts">
  /**
   * Example shortcode usage:
   * [[VideoFeatureBox
   *   heading="Your heading here"
   *   description="A short description goes here."
   *   src1="videos/vid1.mp4"
   *   src2="videos/vid2.mp4"
   *   caption1="Optional caption for video 1"
   *   caption2="Optional caption for video 2"
   *   color="#1a1a2e"
   *   bordercolor="#ffffff"
   * ]]
   */

  import { base } from '$app/paths';

  export let heading: string = '';
  export let description: string = '';
  export let src1: string = '';
  export let src2: string = '';
  export let caption1: string = '';
  export let caption2: string = '';
  export let color: string = '#533b4d';
  export let bordercolor: string = 'rgba(255,255,255,0.3)';

 function goFullscreen(e: MouseEvent) {
  const video = e.currentTarget as HTMLVideoElement;
  video.muted = false;
  if (video.requestFullscreen) {
    video.requestFullscreen();
  }

  function handleFullscreenChange() {
    if (!document.fullscreenElement) {
      video.muted = true;
      document.removeEventListener('fullscreenchange', handleFullscreenChange);
    }
  }

  document.addEventListener('fullscreenchange', handleFullscreenChange);
}

</script>

<div class="vfb-outer">
  <div class="vfb-box" style="background: {color}; border-color: {bordercolor};">

    <!-- Left column: heading + description -->
    <div class="vfb-left">
      {#if heading}
        <h2 class="vfb-heading">{heading}</h2>
      {/if}
      {#if description}
        <p class="vfb-description">{description}</p>
      {/if}
    </div>

    <!-- Right columns: two vertical videos -->
    <div class="vfb-videos">

      {#if src1}
        <div class="vfb-video-col">
          <div class="vfb-phone">
            <img src="{base}/photos/verticalframe.png" alt="" class="vfb-frame" aria-hidden="true" />
            <div class="vfb-video-area">
              <video
                src="{base}/{src1}"
                autoplay
                muted
                loop
                playsinline
                disablepictureinpicture
                on:click={goFullscreen}
                style="cursor: pointer;"
              ></video>
            </div>
          </div>
          {#if caption1}
            <p class="vfb-caption">{caption1}</p>
          {/if}
        </div>
      {/if}

      {#if src2}
        <div class="vfb-video-col">
          <div class="vfb-phone">
            <img src="{base}/photos/verticalframe.png" alt="" class="vfb-frame" aria-hidden="true" />
            <div class="vfb-video-area">
              <video
                src="{base}/{src2}"
                autoplay
                muted
                loop
                playsinline
                disablepictureinpicture
                on:click={goFullscreen}
                style="cursor: pointer;"
              ></video>
            </div>
          </div>
          {#if caption2}
            <p class="vfb-caption">{caption2}</p>
          {/if}
        </div>
      {/if}

    </div>
  </div>
</div>

<style>
  /* Breaks out of text column but not full bleed */
  .vfb-outer {
    width: min(90vw, 900px);
    margin-left: 50%;
    transform: translateX(-50%);
    padding: 2rem 0;
  }

  .vfb-box {
    display: flex;
    flex-direction: row;
    align-items: center;
    gap: 2rem;
    padding: 3rem;
    border: 1px solid;
    border-radius: 2px;
  }

  /* Left column: takes up ~35% */
  .vfb-left {
    flex: 0 0 32%;
    display: flex;
    flex-direction: column;
    gap: 1rem;
  }

  .vfb-heading {
    color: #fff;
    font-size: clamp(2rem, 3vw, 2.5rem);
    letter-spacing: 0.02em;
    font-weight: 700;
    line-height: 1.2;
    margin: 0;
    padding-bottom: 0.75rem;
    border-bottom: 2px solid rgba(255, 255, 255, 0.3)
  }

  .vfb-description {
    color: rgba(255, 255, 255, 0.75);
    font-size: 0.95rem;
    line-height: 1.6;
    margin: 0;
  }

  /* Right side: two video columns side by side */
  .vfb-videos {
    flex: 1 1 0;
    display: flex;
    flex-direction: row;
    gap: 1.5rem;
    align-items: flex-start;
    justify-content: center;
  }

  .vfb-video-col {
    flex: 1 1 0;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 0.75rem;
  }

  /* Phone frame */
  .vfb-phone {
    position: relative;
    width: 100%;
    max-width: 160px;
  }

  .vfb-frame {
    width: 100%;
    display: block;
    pointer-events: none;
    user-select: none;
  }

  /* Video sits inside frame — tweak to match your verticalframe.png */
  .vfb-video-area {
    position: absolute;
    top: 4.5%;
    left: 8%;
    width: 84%;
    height: 91%;
    overflow: hidden;
    border-radius: 0px;
  }

  .vfb-video-area video {
    width: 100%;
    height: 100%;
    object-fit: cover;
    display: block;
  }

  .vfb-caption {
    color: rgba(255, 255, 255, 0.5);
    font-size: 0.75rem;
    text-align: center;
    margin: 0;
    line-height: 1.4;
  }

  /* Mobile: stack vertically */
  @media (max-width: 767px) {
    .vfb-box {
      flex-direction: column;
    }

    .vfb-left {
      flex: none;
      width: 100%;
    }

    .vfb-videos {
      width: 100%;
    }
  }
</style>