<script lang="ts">
	import { navigationIsDelayed } from '$lib/stores/navigationIsDelayed';
	import { fade } from 'svelte/transition';
</script>

<!-- @component
  Show a loading indicator when navigating take too long
-->
{#if Boolean($navigationIsDelayed)}
	<div
		class="fixed z-[999] flex h-screen w-screen items-center justify-center bg-basis opacity-70"
		in:fade={{ duration: 150 }}
	>
		<!-- Loading spinner -->
		<div
			class="h-12 w-12 animate-spin rounded-full border-y-8 border-solid border-accent border-t-transparent"
		/>
	</div>
{/if}
