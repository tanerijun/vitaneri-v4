<script lang="ts">
	export let href: string;
	export let title: string;
</script>

<a
	{href}
	{title}
	target="_blank"
	rel="noreferrer"
	class="flex items-center justify-center transition-colors hover:text-text">
	<slot />
</a>
