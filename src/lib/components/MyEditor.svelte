<script>
    import { Bold, Italic, Link, Code, Bot, WandSparkles } from 'lucide-svelte';

    let textArea; // Reference for the textarea
    let textAreaContent = $state(''); // Bind the textarea value
    const contentLenght = $derived(textAreaContent.length);
    let selectionStart = $state(0);
    let selectionEnd = $state(0);
    let cursorPosition = $state(0);
    let selectedText = $state('');

    // Insert tags into the textarea
    function insert(startTag, endTag) {
        selectionStart = textArea.selectionStart;
        selectionEnd = textArea.selectionEnd;
        const oldText = textAreaContent;

        const prefix = oldText.substring(0, selectionStart);
        const inserted = startTag + oldText.substring(selectionStart, selectionEnd) + endTag;
        const suffix = oldText.substring(selectionEnd);
        textAreaContent = `${prefix}${inserted}${suffix}`;

        // Update the cursor selection
        const newSelectionStart = selectionStart + startTag.length;
        const newSelectionEnd = selectionEnd + startTag.length;
        console.log(newSelectionStart);
        console.log(newSelectionEnd);
        textArea.focus();
        textArea.setSelectionRange(newSelectionStart, newSelectionEnd);

        //textArea.focus();
    }

    // Insert a hyperlink
    function insertURL() {
        const newURL = prompt('Enter the full URL for the link');
        if (newURL) {
            insert(`[${newURL}](`, `)`);
        } else {
            textArea.focus();
        }
    }

    function updateCursorAndSelection(event) {
        selectionStart = textArea.selectionStart;
        selectionEnd = textArea.selectionEnd;
        selectedText = textArea.value.substring(selectionStart, selectionEnd);

        // If no text is selected, the cursor position is the same as selectionStart
        cursorPosition = selectionStart;
    }
</script>

<div class="border-base-300 bg-base-200 mb-4 w-full rounded-lg border p-1">
    <div class="flex justify-between pb-1">
        <div class="join">
            <button class="btn join-item" onclick={() => insert('**', '**')} id="format-strong">
                <Bold />
            </button>
            <button class="btn join-item" onclick={() => insert('*', '*')} id="format-em">
                <Italic />
            </button>
            <button class="btn join-item" onclick={insertURL} id="format-link">
                <Link />
            </button>
            <button
                class="btn join-item"
                onclick={() => insert('\n```\n', '\n```\n')}
                id="format-code"
            >
                <Code />
            </button>
            <div class="dropdown dropdown-bottom join-item">
                <div tabindex="0" role="button" class="btn m-0"><Bot /></div>
                <ul
                    tabindex="0"
                    class="dropdown-content menu bg-base-100 rounded-box z-1 w-52 p-2 shadow-sm"
                >
                    <li>
                        <button
                            class="btn"
                            onclick={() => insert('\n```\n', '\n```\n')}
                            id="format-code"
                        >
                            AA
                        </button>
                    </li>
                    <li><a>Translate</a></li>
                </ul>
            </div>
            <div class="dropdown dropdown-bottom join-item">
                <div tabindex="0" role="button" class="btn m-0"><WandSparkles /></div>
                <ul
                    tabindex="0"
                    class="dropdown-content menu bg-base-100 rounded-box z-1 w-52 p-2 shadow-sm"
                >
                    <li><a>Generate text</a></li>
                    <li><a>Rewrite</a></li>
                </ul>
            </div>
        </div>

        <div class="fieldset-label">{textAreaContent.length}</div>
        <div class="fieldset-label">aabb</div>

        <div>
            <button
                class="btn join-item"
                onclick={() => insert('\n```\n', '\n```\n')}
                id="format-code"
            >
                <Code />
            </button>
        </div>
    </div>
    <div class="rounded-b-lg bg-white px-4 py-2 dark:bg-gray-800">
        <label for="editor" class="sr-only">Publish post</label>
        <textarea
            id="editor"
            bind:this={textArea}
            bind:value={textAreaContent}
            oninput={updateCursorAndSelection}
            onselect={updateCursorAndSelection}
            onkeyup={updateCursorAndSelection}
            onmouseup={updateCursorAndSelection}
            rows="8"
            class="textarea-xl block w-full border-0 bg-white px-0 text-sm text-gray-800 focus:ring-0 dark:bg-gray-800 dark:text-white dark:placeholder-gray-400"
            placeholder="Write an article..."
            required
        ></textarea>
    </div>
    <button class="btn btn-primary btn-ghost mt-1"> Publish post </button>
</div>
<div class="w-full">
    <p>Content length: {contentLenght}</p>
    <p>Selection Start: {selectionStart}</p>
    <p>Selection End: {selectionEnd}</p>
    <p>Selected Text: {selectedText}</p>
    <p>Cursor position: {cursorPosition}</p>
</div>
