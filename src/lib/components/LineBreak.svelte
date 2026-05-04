<script lang="ts">
	import { base } from '$app/paths';

	/**
	 * Example shortcode usage:
	 * [[LineBreak type="rectangle"]]
	 */

	export let type: string | undefined;
</script>

{#if type}
	{#if type === 'rectangle'}
		<figure style="max-width:100px; width:100%">
            <img src='{base}/photos/pinkrectangle.png' alt='' style="width:100%"/>
        </figure> 
	{:else if type === 'dots'}
     <figure style="max-width:60px; width:100%; padding-top: 2rem; padding-bottom: 2rem;">
            <img src='{base}/photos/3dots.png' alt='' style="width:100%"/>
        </figure> 
	{/if}
{/if}