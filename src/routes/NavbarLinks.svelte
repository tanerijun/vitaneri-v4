<script lang="ts">
	import { browser } from '$app/environment';
	import { page } from '$app/stores';
	import { capitalizeFirstLetterOfWord } from '$lib/utils';
	import { onMount } from 'svelte';

	export let isMobile = false;

	let routes = ['posts', 'tags', 'TILs', 'projects', 'about'];

	let marker: HTMLDivElement;
	let activeLink: HTMLElement | null | undefined;

	$: $page, updateActiveLink();
	$: activeLink, updateMarkerPosition();

	onMount(() => {
		// Call updateActiveLink again afterMount
		// because beforeMount, the `linkElement` in updateActiveLink will return null
		// even though the `link` is valid.
		updateActiveLink();
	});

	// Either set activeLink to a mark-able linkElement or null
	function updateActiveLink() {
		if (!browser) return;
		const link = $page.url.pathname.split('/')[1];
		const linkElement = document.getElementById(link);
		activeLink = linkElement;
	}

	function updateMarkerPosition() {
		if (!marker) return; // return early if marker haven't been rendered
		if (activeLink) {
			marker.style.left = activeLink.offsetLeft + 'px';
			marker.style.width = activeLink.offsetWidth + 'px';
			return;
		}
		marker.style.left = 0 + 'px';
		marker.style.width = 0 + 'px';
	}
</script>

<div
	id="marker"
	bind:this={marker}
	class={`absolute ${
		isMobile ? 'top-7' : 'top-8'
	} left-0 h-0.5 w-0 rounded-full bg-accent transition-all`}
/>
{#each routes as route (route)}
	<!-- id attribute is necessary for marker to work -->
	<a
		id={`${route.toLowerCase()}`}
		class:text-text={$page.url.pathname.includes(route)}
		class="relative font-heading text-sm hover:text-text md:text-base"
		href={`/${route.toLowerCase()}`}
	>
		{capitalizeFirstLetterOfWord(route)}
	</a>
{/each}
