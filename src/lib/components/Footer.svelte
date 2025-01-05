<script lang="ts">
    import { Github, NotebookPen, Twitter } from 'lucide-svelte';
</script>

<footer class="footer sm:footer-horizontal bg-neutral text-neutral-content p-10">
    <aside>
        <NotebookPen />
        <p>
            Forgeable.
            <br />
            Forgeable is a powerful and extensible text editor built with Svelte, combining traditional
            formatting tools with cutting-edge AI capabilities.
        </p>
    </aside>
    <nav>
        <h6 class="footer-title">Social</h6>
        <div class="grid grid-flow-col gap-4">
            <a href="https://x.com/RmeetsH">
                <Twitter />
            </a>
            <a href="https://github.com/roberto-butti">
                <Github />
            </a>
        </div>
    </nav>
</footer>
