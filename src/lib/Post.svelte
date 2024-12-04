<script lang="ts">
	import type { UserPost } from '$lib';
	import { Heart, Trash2 } from 'lucide-svelte';

	export let post: UserPost;
	export let onUpdate: () => void;
	export let onRemove: (post: UserPost) => void;

	function likePost() {
		post.likes++;
		onUpdate();
	}
</script>

<div class="border border-gray-200 rounded-lg p-4">
	<div>{post.text}</div>
	<div class="flex justify-between items-center mt-2">
		<div class="text-sm text-gray-500">
			{new Date(post.timestamp).toLocaleString()}
		</div>
		<div class="flex items-center gap-2">
			<button
				class="flex items-center gap-1 text-gray-600 hover:text-red-500"
				on:click={() => onRemove(post)}
			>
				<Trash2 size={16} />
			</button>
			<button class="flex items-center gap-1 text-gray-600 hover:text-red-500" on:click={likePost}>
				<Heart size={16} fill="currentColor" />
				<span class="select-none">{post.likes}</span>
			</button>
		</div>
	</div>
</div>
