<script lang="ts">
	import { onMount, tick, getContext } from 'svelte';

	const i18n = getContext('i18n');

	import ShortcutsModal from '../chat/ShortcutsModal.svelte';
	import Tooltip from '../common/Tooltip.svelte';
	import HelpMenu from './Help/HelpMenu.svelte';

	let showShortcuts = false;
</script>

<div class=" hidden lg:flex fixed bottom-0 right-0 px-1 py-1 z-20">
	
			<a
				class="text-[#00e8ba] w-8 bg-[#003832] font-extrabold flex items-center justify-center text-[1.2rem] rounded-full cursor-pointer mr-2 mb-2"
				href="https://www.intellilab.ch/how-to-intellichat"
				target="_blank"
			>
				?
			</a>
</div>

<ShortcutsModal bind:show={showShortcuts} />
