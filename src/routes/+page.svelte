<script>
	// @ts-nocheck
	import { defaultEvmStores } from 'ethers-svelte';

	import { onMount } from 'svelte';
	import Grid from 'svelte-grid';
	import gridHelp from 'svelte-grid/build/helper/index.mjs';
	import { popup } from '@skeletonlabs/skeleton';
	import { stringify } from 'postcss';

	import ListInfoKeyValue from '../lib/components/ListInfoKeyValue.svelte';
	import WalletConnectEther from '../lib/components/WalletConnectEther.svelte';

	// rfq
	import TableTabs from '../lib/modules/TableTabs.svelte';
	import TableComponent from '../lib/components/TableComponent.svelte';
	import FormComponent from '../lib/components/FormComponent.svelte';
	import BestPrice from '../lib/components/BestPrice.svelte';

	import { itemsConfig } from '../lib/config/layoutConfig';

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

	onMount(() => {
		defaultEvmStores.setProvider();
	});

	let items = itemsConfig.map((item) => {
		return {
			6: gridHelp.item({
				...item.coordinates,
				fixed: item.fixed
			}),
			id: id(),
			com: componentsMap[item.com],
			canRemove: item.canRemove,
			name: item.name
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
		<svelte:component this={dataItem.com} name={dataItem.name}></svelte:component>
	</Grid>
</div>

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
