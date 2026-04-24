<script lang="ts">
	/**
	 * Example shortcode usage:
	 * [[CoolNewFeature src="https://example.com/embed" size="large" credit="Name" title="Optional title"]]
	 */

	export let src1: string | undefined;
    export let src2: string | undefined;
	export let size: 'large' | 'fit' = 'large';
	export let credit1: string | undefined;
    export let credit2: string | undefined;
	export let title: string = '';
    export let alt1: string = '';
    export let alt2: string = '';
</script>

{#if src1 && src2}
	{#if size === 'full'}
        <div class="row full-bleed">
            <figure class="my-3 col-12 col-sm-6">
                <img src="{src1}" alt="{alt1}" class="img-fluid d-block">
                {#if credit1}
                    <figcaption class="figure-caption text-muted mt-2">credit: {credit1}</figcaption>
                {/if}
            </figure>
            <figure class="my-3 col-12 col-sm-6">
                <img src="{src2}" alt="{alt2}" class="img-fluid d-block">
                {#if credit2}
                    <figcaption class="figure-caption text-muted mt-2">credit: {credit2}</figcaption>
                {/if}
            </figure>
        </div>
	{:else if size === 'large'}
        <div class="row full-bleed justify-content-center">
            <div class="col-12 col-lg-10">
                <div class="container-fluid">
                    <div class="row justify-content-center">

                        <!-- IMAGE 1 -->
                        <figure class="my-3 col-12 col-md-10 col-lg-6">
                            <img src="{src1}" alt="{alt1}" class="img-fluid d-block">
                            {#if credit1}
                                <figcaption class="figure-caption text-muted mt-2">credit: {credit1}</figcaption>
                            {/if}
                        </figure>

                        <!-- IMAGE 2 -->
                        <figure class="my-3 col-12 col-md-10 col-lg-6">
                            <img src="{src2}" alt="{alt2}" class="img-fluid d-block">
                            {#if credit2}
                                <figcaption class="figure-caption text-muted mt-2">credit: {credit2}</figcaption>
                            {/if}
                        </figure>
                    </div>
                </div>
            </div>
        </div>
    {/if}
{/if}
