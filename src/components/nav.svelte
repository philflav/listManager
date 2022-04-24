<script>
	// @ts-nocheck

	import { user, isLoggedIn } from '../stores/stores';
</script>

<nav
	class="relative w-full flex flex-wrap items-center justify-around py-2 bg-gray-900 text-gray-100"
>
	<a class="nav-link text-white opacity-80 hover:opacity-100 focus:opacity-100 p-0" href="/">Home</a
	>
	<!-- <a class="nav-link text-white opacity-80 hover:opacity-100 focus:opacity-100 p-0" href="/profile"
		>Profile</a
	> -->
	<a class="nav-link text-white opacity-80 hover:opacity-100 focus:opacity-100 p-0" href="/todo"
		>Todo List</a
	>
	<a
		class="nav-link text-white opacity-80 hover:opacity-100 focus:opacity-100 p-0"
		href="/shopping_list">Shopping List</a
	>
	<a
		class="nav-link text-white opacity-80 hover:opacity-100 focus:opacity-100 p-0"
		href="/bucketList">Bucket List</a
	>
	<!-- <a class="nav-link text-white opacity-80 hover:opacity-100 focus:opacity-100 p-0" href="/about"
		>About</a
	> -->
	{#if !$isLoggedIn}
		<a class="nav-link text-white opacity-80 hover:opacity-100 focus:opacity-100 p-0" href="/login"
			>Login</a
		>
	{/if}
	{#if $isLoggedIn}
		<a class="nav-link text-white opacity-80 hover:opacity-100 focus:opacity-100 p-0" href="/login"
			>Logout</a
		>
		<img class="image" width="40" height="40" src={$user.photoURL} alt="user" />
	{/if}
</nav>

<style>
	.image {
		border-radius: 100%;
		margin-bottom: 10px;
	}
</style>
