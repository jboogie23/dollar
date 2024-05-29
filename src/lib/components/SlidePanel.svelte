<script lang="ts">
	import { createEventDispatcher } from 'svelte';
	import Portal from '$lib/components/Portal.svelte';
	import Overlay from './Overlay.svelte';
	import Arrow from '$lib/components/Icon/Arrow.svelte';

	const dispatch = createEventDispatcher();
</script>

<svelte:window
	on:keydown={(e) => {
		if (e.key === 'Escape') {
			dispatch('closePanel');
		}
	}}
/>

<Portal>
	<Overlay />
	<div
		class="fixed right-0 top-0 z-slidePanel h-screen w-3/4 overflow-y-scroll bg-white px-32 py-20 shadow-slidePanel"
	>
		<button
			class="absolute left-7 top-5 text-pastelPurple hover:text-daisyBush"
			on:click={() => dispatch('closePanel')}
		>
			<Arrow />
		</button>
		<slot />
	</div>
</Portal>
