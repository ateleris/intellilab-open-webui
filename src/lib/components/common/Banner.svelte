<script lang="ts">
	import type { Banner } from '$lib/types';
	import { onMount, createEventDispatcher } from 'svelte';
	import { fade } from 'svelte/transition';
	import DOMPurify from 'dompurify';
	import { marked } from 'marked';

	const dispatch = createEventDispatcher();

	export let banner: Banner = {
		id: '',
		type: 'info',
		title: '',
		content: '',
		url: '',
		dismissable: true,
		timestamp: Math.floor(Date.now() / 1000)
	};
	export let className = 'mx-4';

	export let dismissed = false;

	let mounted = false;

	const classNames: Record<string, string> = {
		info: 'bg-blue-500/20 text-blue-700 dark:text-blue-200 ',
		success: 'bg-green-500/20 text-green-700 dark:text-green-200',
		warning: 'bg-yellow-500/20 text-yellow-700 dark:text-yellow-200',
		error: 'bg-red-500/20 text-red-700 dark:text-red-200'
	};

	const dismiss = (id) => {
		dismissed = true;
		dispatch('dismiss', id);
	};

	onMount(() => {
		mounted = true;
	});
	// Reaktive Variable zur Bereinigung des Bannerinhalts
	$: cleanContent = DOMPurify.sanitize(banner.content, {
		ALLOWED_TAGS: ['iframe', 'div', 'span', 'h3', 'p', 'img', 'a'], // Weitere Tags ggf. hinzufügen
		ALLOWED_ATTR: ['src', 'href', 'alt', 'class', 'style', 'frameborder', 'allow', 'allowfullscreen']
	});
</script>

{#if !dismissed}
	{#if mounted}
	<div class="relative mx-3" transition:fade={{ delay: 100, duration: 300 }}>
			{@html marked.parse(cleanContent)}
			{#if banner.dismissible}
				<button
					on:click={() => dismiss(banner.id)}
					class="absolute right-3 top-3 opacity-70 hover:opacity-100 text-gray-500"
				>&times;</button>



			{/if}
		</div>
	{/if}
{/if}
