<script lang="ts">
	import { createEventDispatcher } from 'svelte';
	import Cancel from './Icon/Cancel.svelte';
	import Overlay from './Overlay.svelte';
	import Portal from './Portal.svelte';

	export let isVisible: boolean = false;
	const dispatch = createEventDispatcher();
</script>

<svelte:window
	on:keydown={(e) => {
		if (e.key === 'Escape') {
			dispatch('close');
		}
	}}
/>

{#if isVisible}
	<Portal>
		<Overlay />
		<div class="center fixed inset-0 z-modal">
			<div class="relative min-h-[230px] w-full max-w-[450px] rounded bg-white px-10 py-7">
				<button
					on:click={() => dispatch('close')}
					class="absolute right-4 top-4 text-pastelPurple hover:text-daisyBush"
					><Cancel />
				</button>
				<slot />
			</div>
		</div>
	</Portal>
{/if}
