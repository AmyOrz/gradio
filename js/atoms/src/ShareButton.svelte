<script lang="ts">
	import IconButton from "./IconButton.svelte";
	import { createEventDispatcher } from "svelte";
	import type { ShareData } from "@gradio/utils";
	import { ShareError } from "@gradio/utils";
	``;
	const dispatch = createEventDispatcher<{
		share: ShareData;
		error: string;
	}>();

	export let formatter: (arg0: any) => Promise<string>;
	export let value: any;
	let pending = false;
</script>

<IconButton
	Icon={null}
	label="Share"
	{pending}
	on:click={async () => {
		try {
			pending = true;
			const formatted = await formatter(value);
			dispatch("share", {
				description: formatted
			});
		} catch (e) {
			console.error(e);
			let message = e instanceof ShareError ? e.message : "Share failed.";
			dispatch("error", message);
		} finally {
			pending = false;
		}
	}}
/>
