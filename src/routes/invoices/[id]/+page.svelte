<script lang="ts">
	export let data;
	console.log(data);
	import Dashboard from '$lib/layouts/Dashboard.svelte';
</script>

{data.id}
<Dashboard>Invoice</Dashboard>
