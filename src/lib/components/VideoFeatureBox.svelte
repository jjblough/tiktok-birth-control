<script lang="ts">
  import { base } from '$app/paths';

  export let heading: string = '';
  export let description: string = '';
  export let src1: string = '';
  export let src2: string = '';
  export let caption1: string = '';
  export let caption2: string = '';
  export let color: string = '#533b4d';
  export let bordercolor: string = 'rgba(255,255,255,0.3)';

  let videoEl1: HTMLVideoElement;
  let videoEl2: HTMLVideoElement;
  let paused1 = true;
  let paused2 = true;
  let muted1 = true;
  let muted2 = true;

  function goFullscreen(e: MouseEvent) {
    const video = e.currentTarget as HTMLVideoElement;
    video.muted = false;
    
    if (video.requestFullscreen) {
      video.requestFullscreen().then(() => {
        video.style.width = '100%';
        video.style.height = '100%';
        video.style.objectFit = 'contain';
        video.style.backgroundColor = '#000';
      });
    }

    function handleFullscreenChange() {
      if (!document.fullscreenElement) {
        video.muted = true;
        video.style.objectFit = 'cover';
        video.style.width = '';
        video.style.height = '';
        document.removeEventListener('fullscreenchange', handleFullscreenChange);
      }
    }

    document.addEventListener('fullscreenchange', handleFullscreenChange);
  }

  function togglePlay(videoEl: HTMLVideoElement, idx: number) {
    if (!videoEl) return;
    if (videoEl.paused) {
      videoEl.play();
      if (idx === 1) paused1 = false;
      else paused2 = false;
    } else {
      videoEl.pause();
      if (idx === 1) paused1 = true;
      else paused2 = true;
    }
  }

  function toggleMute(videoEl: HTMLVideoElement, idx: number) {
    if (!videoEl) return;
    videoEl.muted = !videoEl.muted;
    if (idx === 1) muted1 = videoEl.muted;
    else muted2 = videoEl.muted;
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
            <img src="{base}/photos/verticalframe.png" alt="" loading="lazy" class="vfb-frame" aria-hidden="true" />
            
            <div class="vfb-video-area">
              <video
                bind:this={videoEl1}
                src="{base}/{src1}"
                autoplay
                muted
                loop
                playsinline
                disablepictureinpicture
                preload="none"
                on:click={goFullscreen}
                style="cursor: pointer;"
              ></video>
            </div>

            <!-- Custom controls for video 1 -->
            <div class="vfb-controls">
              <button class="vfb-btn" on:click={() => togglePlay(videoEl1, 1)} aria-label={paused1 ? 'Play' : 'Pause'}>
                {#if paused1}
                  <svg viewBox="0 0 24 24" fill="currentColor" width="20" height="20">
                    <path d="M8 5v14l11-7z" />
                  </svg>
                {:else}
                  <svg viewBox="0 0 24 24" fill="currentColor" width="20" height="20">
                    <path d="M6 19h4V5H6v14zm8-14v14h4V5h-4z" />
                  </svg>
                {/if}
              </button>

              <button class="vfb-btn" on:click={() => toggleMute(videoEl1, 1)} aria-label={muted1 ? 'Unmute' : 'Mute'}>
                {#if muted1}
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
                bind:this={videoEl2}
                src="{base}/{src2}"
                autoplay
                muted
                loop
                playsinline
                disablepictureinpicture
                preload="none"
                on:click={goFullscreen}
                style="cursor: pointer;"
              ></video>
            </div>

            <!-- Custom controls for video 2 -->
            <div class="vfb-controls">
              <button class="vfb-btn" on:click={() => togglePlay(videoEl2, 2)} aria-label={paused2 ? 'Play' : 'Pause'}>
                {#if paused2}
                  <svg viewBox="0 0 24 24" fill="currentColor" width="20" height="20">
                    <path d="M8 5v14l11-7z" />
                  </svg>
                {:else}
                  <svg viewBox="0 0 24 24" fill="currentColor" width="20" height="20">
                    <path d="M6 19h4V5H6v14zm8-14v14h4V5h-4z" />
                  </svg>
                {/if}
              </button>

              <button class="vfb-btn" on:click={() => toggleMute(videoEl2, 2)} aria-label={muted2 ? 'Unmute' : 'Mute'}>
                {#if muted2}
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
    max-width: 220px;
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

  /* Custom controls */
  .vfb-controls {
    position: absolute;
    bottom: 8%;
    left: 0;
    width: 100%;
    display: flex;
    justify-content: space-between;
    padding: 0 12%;
    pointer-events: none;
  }

  .vfb-btn {
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

  .vfb-btn:hover {
    background: rgba(0, 0, 0, 0.8);
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