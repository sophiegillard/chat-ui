<script lang="ts">
	import { PUBLIC_APP_NAME } from "$env/static/public";
	import { PUBLIC_ANNOUNCEMENT_BANNERS } from "$env/static/public";
	import { createEventDispatcher } from "svelte";
	import CarbonArrowUpRight from "~icons/carbon/arrow-up-right";
	import AnnouncementBanner from "../AnnouncementBanner.svelte";
	import ModelsModal from "../ModelsModal.svelte";
	import type { Model } from "$lib/types/Model";
	import type { LayoutData } from "../../../routes/$types";
	import { findCurrentModel } from "$lib/utils/models";
	import SmallLogoGreen from "../icons/small_logo_green.png";
	import SmallLogoBlue from "../icons/small_logo_blue.png";

	export let settings: LayoutData["settings"];
	export let models: Model[];

	let isModelsModalOpen = false;

	$: currentModelMetadata = findCurrentModel(models, settings.activeModel);

	const announcementBanners = PUBLIC_ANNOUNCEMENT_BANNERS
		? JSON.parse(PUBLIC_ANNOUNCEMENT_BANNERS)
		: [];

	const dispatch = createEventDispatcher<{ message: string }>();
</script>

<div class="my-auto grid gap-8 lg:grid-cols-3">
	<div class="lg:col-span-1">
		<div>
			<div class="font-industry mb-3 flex items-end text-3xl font-semibold">
				<!-- svelte-ignore a11y-img-redundant-alt -->
				<img src={SmallLogoGreen} alt="Logo" class=" mr-1 hidden w-12 flex-none dark:block" />
				<img src={SmallLogoBlue} alt="Logo" class=" mr-1 w-12 flex-none dark:hidden" />

				{PUBLIC_APP_NAME}
			</div>
			<p class="text-base text-gray-600 dark:text-gray-400">
				Making the community's best AI chat models available to everyone.
			</p>
		</div>
	</div>
	<div class="lg:col-span-2 lg:pl-24">
		{#each announcementBanners as banner}
			<AnnouncementBanner classNames="mb-4" title={banner.title}>
				<a
					target="_blank"
					href={banner.linkHref}
					class="mr-2 flex items-center underline hover:no-underline"
					><CarbonArrowUpRight class="mr-1.5 text-xs" /> {banner.linkTitle}</a
				>
			</AnnouncementBanner>
		{/each}

		{#if isModelsModalOpen}
			<ModelsModal {settings} {models} on:close={() => (isModelsModalOpen = false)} />
		{/if}
	</div>
	{#if currentModelMetadata.promptExamples}
		<div class="lg:col-span-3 lg:mt-12">
			<p class="mb-3 text-gray-600 dark:text-gray-300">Examples</p>
			<div class="grid gap-3 lg:grid-cols-3 lg:gap-5">
				{#each currentModelMetadata.promptExamples as example}
					<button
						type="button"
						class="rounded-xl border bg-gray-50 p-2.5 text-gray-600 hover:bg-gray-100 dark:border-gray-800 dark:bg-gray-800 dark:text-gray-300 dark:hover:bg-gray-700 sm:p-4"
						on:click={() => dispatch("message", example.prompt)}
					>
						{example.title}
					</button>
				{/each}
			</div>
		</div>{/if}
</div>
