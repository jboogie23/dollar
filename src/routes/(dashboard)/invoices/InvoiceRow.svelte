<script lang="ts">
	import AdditonalOptions from '$lib/components/AdditonalOptions.svelte';
	import ThreeDots from '$lib/components/Icon/ThreeDots.svelte';
	import View from '$lib/components/Icon/View.svelte';
	import Tag from '$lib/components/Tag.svelte';
	import { convertDate, isLate } from '$lib/utils/dateHelpers';
	import { centsToDollars, sumLineItems } from '$lib/utils/moneyHelpers';
	import Edit from '$lib/components/Icon/Edit.svelte';
	import Send from '$lib/components/Icon/Send.svelte';
	import Trash from '$lib/components/Icon/Trash.svelte';
	import Modal from '$lib/components/Modal.svelte';
	import Button from '$lib/components/Button.svelte';
	import { deleteInvoice } from '$lib/stores/InvoiceStore';

	export let invoice: Invoice;
	let isAdditionalMenuShowing = false;
	let isOptionsDisabled = false;
	let isModalShowing = false;

	const handleDelete = () => {
		isModalShowing = true;
		isAdditionalMenuShowing = false;
		console.log('deleting');
	};
	const handleEdit = () => {
		console.log('editing');
	};
	const handleSendInvoice = () => {
		console.log('sending');
	};

	const getInvoiceLabel = () => {
		if (invoice.invoiceStatus === 'draft') {
			return 'draft';
		} else if (invoice.invoiceStatus === 'sent' && !isLate(invoice.dueDate)) {
			isOptionsDisabled = true;
			return 'sent';
		} else if (invoice.invoiceStatus === 'sent' && isLate(invoice.dueDate)) {
			isOptionsDisabled = true;
			return 'late';
		} else if (invoice.invoiceStatus === 'paid') {
			isOptionsDisabled = true;
			return 'paid';
		}
	};
</script>

<div
	class="invoice-table invoice-row items-center rounded-lg bg-white py-3 shadow-tableRow lg:py-6"
>
	<div class="status"><Tag className="ml-auto lg:ml-0" label={getInvoiceLabel()} /></div>
	<div class="dueDate text-sm lg:text-lg">{convertDate(invoice.dueDate)}</div>
	<div class="invoiceNumber text-sm lg:text-lg">{invoice.invoiceNumber}</div>
	<div class="clientName truncate whitespace-nowrap text-base font-bold lg:text-xl">
		{invoice.client.name}
	</div>
	<div class="amount text-right font-mono text-sm font-bold lg:text-lg">
		${centsToDollars(sumLineItems(invoice.lineItems))}
	</div>
	<div class="center viewButton hidden text-sm lg:flex lg:text-lg">
		<a href="#" class="text-pastelPurple hover:text-daisyBush"><View /></a>
	</div>
	<div class="center moreButton relative hidden text-sm lg:flex lg:text-lg">
		<button
			class=" text-pastelPurple hover:text-daisyBush"
			on:click={() => {
				isAdditionalMenuShowing = !isAdditionalMenuShowing;
			}}><ThreeDots /></button
		>
		{#if isAdditionalMenuShowing}
			<AdditonalOptions
				options={[
					{ label: 'Edit', icon: Edit, onClick: handleEdit, disabled: isOptionsDisabled },
					{ label: 'Delete', icon: Trash, onClick: handleDelete, disabled: false },
					{ label: 'Send', icon: Send, onClick: handleSendInvoice, disabled: isOptionsDisabled }
				]}
			/>
		{/if}
	</div>
</div>

<Modal isVisible={isModalShowing} on:close={() => (isModalShowing = false)}>
	<div class="flex h-full min-h-[175px] flex-col items-center justify-between gap-6">
		<div class="text-center text-xl font-bold text-daisyBush">
			Are you sure you want to delete this invoice to <span class="text-scarlet"
				>{invoice.client.name}</span
			>
			for
			<span class="text-scarlet">${centsToDollars(sumLineItems(invoice.lineItems))}</span>?
		</div>
		<div class="flex gap-4">
			<Button
				isAnimated={false}
				label={'Cancel'}
				onClick={() => {
					isModalShowing = false;
				}}
				style="secondary"
			/>
			<Button
				isAnimated={false}
				label={'Yes, Delete it'}
				onClick={() => {
					deleteInvoice(invoice);
					isModalShowing = false;
				}}
				style="destructive"
			/>
		</div>
	</div>
</Modal>

<style lang="postcss">
	.invoice-row {
		grid-template-areas:
			'invoiceNumber invoiceNumber'
			'clientName amount'
			'dueDate status';
	}

	@media (min-width: 1024px) {
		.invoice-row {
			grid-template-areas: 'status dueDate invoiceNumber clientName amount viewButton moreButton';
		}
	}

	.invoice-row .status {
		grid-area: status;
	}
	.invoice-row .dueDate {
		grid-area: dueDate;
	}
	.invoice-row .invoiceNumber {
		grid-area: invoiceNumber;
	}
	.invoice-row .clientName {
		grid-area: clientName;
	}
	.invoice-row .amount {
		grid-area: amount;
	}
	.invoice-row .viewButton {
		grid-area: viewButton;
	}
	.invoice-row .moreButton {
		grid-area: moreButton;
	}
</style>
