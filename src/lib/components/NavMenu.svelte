<script lang="ts">
	import { base } from "$app/paths";
	import { createEventDispatcher } from "svelte";
	import SmallLogoGreen from "./icons/small_logo_green.png";
	import SmallLogoBlue from "./icons/small_logo_blue.png";
	import { switchTheme } from "$lib/switchTheme";
	import { PUBLIC_APP_NAME, PUBLIC_ORIGIN, PUBLIC_WEBSITE_NAME } from "$env/static/public";
	import NavConversationItem from "./NavConversationItem.svelte";
	import type { LayoutData } from "../../routes/$types";
	import IoLogoFacebook from "svelte-icons/io/IoLogoFacebook.svelte";
	import IoLogoTwitter from "svelte-icons/io/IoLogoTwitter.svelte";
	import IoLogoInstagram from "svelte-icons/io/IoLogoInstagram.svelte";
	import FaDiscord from "svelte-icons/fa/FaDiscord.svelte";

	const dispatch = createEventDispatcher<{
		shareConversation: { id: string; title: string };
		clickSettings: void;
		clickLogout: void;
	}>();

	export let conversations: Array<{
		id: string;
		title: string;
	}> = [];
	export let user: LayoutData["user"];
</script>

<div class="sticky top-0 flex flex-none items-center justify-between px-3 py-3.5 max-sm:pt-0">
	<a
		class="font-industry flex items-end rounded-xl text-lg font-semibold"
		href="{PUBLIC_ORIGIN}{base}/"
	>
		<img src={SmallLogoGreen} alt="Logo" class=" mr-1 hidden w-8 flex-none  dark:block" />
		<img src={SmallLogoBlue} alt="Logo" class=" mr-1 w-8 flex-none dark:hidden" />
		{PUBLIC_APP_NAME}
	</a>
	<a
		href={`${base}/`}
		class="flex rounded-lg border bg-white px-2 py-0.5 text-center shadow-sm hover:shadow-none dark:border-gray-600 dark:bg-gray-700"
	>
		New Chat
	</a>
</div>
<div
	class="scrollbar-custom flex flex-col gap-1 overflow-y-auto rounded-r-xl bg-gradient-to-l from-gray-50 px-3 pb-3 pt-2 dark:from-gray-800/30"
>
	{#each conversations as conv (conv.id)}
		<NavConversationItem on:editConversationTitle on:deleteConversation {conv} />
	{/each}
</div>
<div
	class="mt-0.5 flex flex-col gap-1 rounded-r-xl bg-gradient-to-l from-gray-50 p-3 text-sm dark:from-gray-800/30"
>
	{#if user?.username || user?.email}
		<form
			action="{base}/logout"
			method="post"
			class="group flex items-center gap-1.5 rounded-lg pl-3 pr-2 hover:bg-gray-100 dark:hover:bg-gray-700"
		>
			<span
				class="flex h-9 flex-none shrink items-center gap-1.5 truncate pr-2 text-gray-500 dark:text-gray-400"
				>{user?.username || user?.email}</span
			>
			<button
				type="submit"
				class="ml-auto h-6 flex-none items-center gap-1.5 rounded-md border bg-white px-2 text-gray-700 shadow-sm hover:shadow-none group-hover:flex dark:border-gray-600 dark:bg-gray-600 dark:text-gray-400 dark:hover:text-gray-300 md:hidden"
			>
				Sign Out
			</button>
		</form>
	{/if}
	<button
		on:click={switchTheme}
		type="button"
		class="flex h-9 flex-none items-center gap-1.5 rounded-lg pl-3 pr-2 text-gray-500 hover:bg-gray-100 dark:text-gray-400 dark:hover:bg-gray-700"
	>
		Theme
	</button>

	<button
		on:click={() => dispatch("clickSettings")}
		type="button"
		class="flex h-9 flex-none items-center gap-1.5 rounded-lg pl-3 pr-2 text-gray-500 hover:bg-gray-100 dark:text-gray-400 dark:hover:bg-gray-700"
	>
		Settings
	</button>
	{#if PUBLIC_APP_NAME === "HuggingChat"}
		<a
			href="https://huggingface.co/spaces/huggingchat/chat-ui/discussions"
			target="_blank"
			rel="noreferrer"
			class="flex h-9 flex-none items-center gap-1.5 rounded-lg pl-3 pr-2 text-gray-500 hover:bg-gray-100 dark:text-gray-400 dark:hover:bg-gray-700"
		>
			Feedback
		</a>
		<a
			href="{base}/privacy"
			class="flex h-9 flex-none items-center gap-1.5 rounded-lg pl-3 pr-2 text-gray-500 hover:bg-gray-100 dark:text-gray-400 dark:hover:bg-gray-700"
		>
			About & Privacy
		</a>
	{/if}
	<div
		class="flex h-9 flex-none items-center gap-1.5 rounded-lg pl-3 pr-2 text-gray-500 dark:text-gray-400"
	>
		<a
			class="flex h-6 flex-none items-center gap-1.5 rounded-lg pl-3 pr-2 text-gray-500 hover:bg-gray-100 dark:text-gray-400 dark:hover:bg-gray-700"
			target="_blank"
			href={`https://www.facebook.com/sharer/sharer.php?u=https://www.arescreative.fr/&amp;src=sdkpreparse`}
			><IoLogoFacebook /></a
		>
		<a
			class="flex h-6 flex-none items-center gap-1.5 rounded-lg pl-3 pr-2 text-gray-500 hover:bg-gray-100 dark:text-gray-400 dark:hover:bg-gray-700"
			target="_blank"
			href="https://www.instagram.com/"><IoLogoInstagram /></a
		>
		<a
			class="flex h-6 flex-none items-center gap-1.5 rounded-lg pl-3 pr-2 text-gray-500 hover:bg-gray-100 dark:text-gray-400 dark:hover:bg-gray-700"
			target="_blank"
			href="https://discord.com/"><FaDiscord /></a
		>
		<a
			class="flex h-6 flex-none items-center gap-1.5 rounded-lg pl-3 pr-2 text-gray-500 hover:bg-gray-100 dark:text-gray-400 dark:hover:bg-gray-700"
			target="_blank"
			href="http://twitter.com/intent/tweet/?url=https://www.arescreative.fr/"><IoLogoTwitter /></a
		>
	</div>
</div>
