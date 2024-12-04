<script lang="ts">
	import type { UserPost } from '$lib';
	import Post from '$lib/Post.svelte';
	import { onMount } from 'svelte';

	let newPostText = '';
	let posts: UserPost[] = [];

	function addPost() {
		if (newPostText.length === 0) return;

		const newPost = { text: newPostText, timestamp: Date.now(), likes: 0 };
		posts = [newPost, ...posts];
		newPostText = '';
		updatePosts();
	}

	let sortBy: 'newest' | 'likes' = 'newest';

	function updatePosts() {
		posts = [...posts].sort((a, b) => {
			if (sortBy === 'newest') {
				return b.timestamp - a.timestamp;
			} else {
				return b.likes - a.likes;
			}
		});

		localStorage.setItem('posts', JSON.stringify(posts));
	}

	function removePost(post: UserPost) {
		posts = posts.filter((p) => p !== post);
		updatePosts();
	}

	onMount(() => {
		const savedPosts = localStorage.getItem('posts');
		if (savedPosts) {
			posts = JSON.parse(savedPosts);
		}
	});
</script>

<div class="w-full justify-center flex flex-col gap-4 p-4">
	<form class="flex gap-4" on:submit={addPost}>
		<input
			class="border border-gray-200 rounded-lg p-2 flex-1"
			type="text"
			bind:value={newPostText}
		/>
		<button type="submit" class="p-2 bg-blue-500 text-white rounded-lg">Add post</button>
	</form>
	<div class="flex justify-between items-center">
		<select bind:value={sortBy} on:change={updatePosts}>
			<option value="newest">Newest</option>
			<option value="likes">Likes</option>
		</select>
	</div>
	{#each posts as post}
		<Post {post} onUpdate={updatePosts} onRemove={removePost} />
	{/each}
</div>
