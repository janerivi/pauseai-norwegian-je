<script lang="ts">
	import Button from '$lib/components/Button.svelte'
	import ExternalLink from '$lib/components/custom/a.svelte'
	import consent from '$lib/components/widget-consent/WidgetConsentStore'
	import loadTwitter from '$lib/components/widget-consent/loadTwitter'

	let wrapper: HTMLDivElement | undefined

	$: {
		if (wrapper) {
			loadTwitter()
			;(window as any).twttr.ready(() => {
				if (wrapper) (window as any).twttr.load(wrapper)
			})
		}
	}
</script>

{#if $consent}
	<div bind:this={wrapper}>
		<slot />
	</div>
{:else}
	<div class="widget-consent">
		<div class="consent-text">
			Ved å tillate X (Twitter) widgets, samtykker du med at data håndteres i følge <ExternalLink href="https://twitter.com/en/privacy">personvern praksisen hos X</ExternalLink>.
		</div>
		<Button
			on:click={() => {
				let wasSet = false
				if ($consent != null) wasSet = true
				$consent = true
				if (wasSet) location.reload()
			}}>Enable X (Twitter) widgets</Button
		>
	</div>
{/if}

<style>
	.widget-consent {
		display: flex;
		flex-direction: column;
		text-align: center;
		align-items: center;
		padding: 1rem;
		border: 3px solid var(--brand);
		border-radius: 3px;
		margin: 1rem 0rem 1rem;
	}

	.consent-text {
		margin: 0rem 0rem 1rem;
	}
</style>
