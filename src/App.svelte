<script>
	// @ts-nocheck
	export const ssr = false;
	export const prerender = false;

	import './app.postcss';
	import { AppShell } from '@skeletonlabs/skeleton';

	// Highlight JS
	import hljs from 'highlight.js/lib/core';
	import 'highlight.js/styles/github-dark.css';
	import { storeHighlightJs } from '@skeletonlabs/skeleton';
	import xml from 'highlight.js/lib/languages/xml'; // for HTML
	import css from 'highlight.js/lib/languages/css';
	import javascript from 'highlight.js/lib/languages/javascript';
	import typescript from 'highlight.js/lib/languages/typescript';

	// Floating UI for Popups
	import { computePosition, autoUpdate, flip, shift, offset, arrow } from '@floating-ui/dom';
	import { storePopup } from '@skeletonlabs/skeleton';

	import ListInfoKeyValue from './lib/components/ListInfoKeyValue.svelte';
	import WalletConnectEther from './lib/components/WalletConnectEther.svelte';

	// rfq
	import TableTabs from './lib/modules/TableTabs.svelte';
	import TableComponent from './lib/components/TableComponent.svelte';
	import FormComponent from './lib/components/FormComponent.svelte';
	import BestPrice from './lib/components/BestPrice.svelte';

	import { itemsConfig } from './lib/config/layoutConfig';

	import Grid from 'svelte-grid';
	import gridHelp from 'svelte-grid/build/helper/index.mjs';
	import { popup } from '@skeletonlabs/skeleton';

	hljs.registerLanguage('xml', xml); // for HTML
	hljs.registerLanguage('css', css);
	hljs.registerLanguage('javascript', javascript);
	hljs.registerLanguage('typescript', typescript);
	storeHighlightJs.set(hljs);
	storePopup.set({ computePosition, autoUpdate, flip, shift, offset, arrow });

	const popupFeatured = {
		event: 'hover',
		target: 'popupHover',
		placement: 'top'
	};

	const id = () => Math.random().toString(36).substr(2, 9);
	const componentsMap = {
		ListInfoKeyValue,
		FormComponent,
		BestPrice,
		TableComponent,
		WalletConnectEther,
		TableTabs
	};

	let items = itemsConfig.map((item) => {
		return {
			6: gridHelp.item({
				...item.coordinates,
				fixed: item.fixed
			}),
			id: id(),
			com: componentsMap[item.com],
			canRemove: item.canRemove
		};
	});

	const cols = [[120, 6]];
	const remove = (item) => {
		items = items.filter((value) => value.id !== item.id);

		if (adjustAfterRemove) {
			items = gridHelp.adjust(items, cols);
		}
	};
	let adjustAfterRemove = false;
</script>

<AppShell>
	<div>
		<Grid bind:items rowHeight={100} let:item let:dataItem {cols}>
			{#if dataItem.canRemove}
				<span
					on:pointerdown={(e) => e.stopPropagation()}
					on:click={() => remove(dataItem)}
					class="remove"
				>
					✕
				</span>
			{/if}
			<svelte:component this={dataItem.com}></svelte:component>
		</Grid>
	</div>
</AppShell>

<style>
	.remove {
		text-align: right;
		color: grey;
		display: block;
		position: absolute;
		right: 9px;
		top: 9px;
		cursor: pointer;
	}
</style>
