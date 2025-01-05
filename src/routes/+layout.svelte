<script lang="ts">
    import Footer from '$lib/components/Footer.svelte';
    import '../app.css';
    let { children } = $props();
</script>

<div class="navbar bg-primary text-primary-content shadow-xl">
    <h1 class="px-4 font-serif text-xl">Forgeable: The Smart Editor</h1>
</div>
<main class="container mx-auto px-4">
    {@render children()}
</main>
<Footer />
