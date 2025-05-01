<script lang="ts">
	import '$lib/styles/global.css';
	import Logo from '$lib/components/icons/logo.svelte';
	import { page } from '$app/state';
	let { children } = $props();
</script>

<header>
	<a href="/">
		<Logo />
	</a>
	<nav>
		<a href="/" aria-current={page.url.pathname === '/' ? 'page' : undefined}>More Tools</a>
	</nav>
</header>

{@render children()}
