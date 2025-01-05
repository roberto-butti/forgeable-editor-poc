<script>
    import { Bold, Italic, Link, Code, Bot, WandSparkles } from 'lucide-svelte';

    let textArea; // Reference for the textarea
    let textAreaContent = ''; // Bind the textarea value

    // Insert tags into the textarea
    function insert(startTag, endTag) {
        const selectionStart = textArea.selectionStart;
        const selectionEnd = textArea.selectionEnd;
        const oldText = textAreaContent;

        const prefix = oldText.substring(0, selectionStart);
        const inserted = startTag + oldText.substring(selectionStart, selectionEnd) + endTag;
        const suffix = oldText.substring(selectionEnd);
        textAreaContent = `${prefix}${inserted}${suffix}`;

        // Update the cursor selection
        const newSelectionStart = selectionStart + startTag.length;
        const newSelectionEnd = selectionEnd + startTag.length;
        textArea.setSelectionRange(newSelectionStart, newSelectionEnd);

        textArea.focus();
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
</script>

<div
    class="mb-4 w-full rounded-lg border border-gray-200 bg-gray-50 dark:border-gray-600 dark:bg-gray-700"
>
    <div class="flex items-center justify-between border-b px-3 py-2 dark:border-gray-600">
        <div
            class="flex flex-wrap items-center divide-gray-200 sm:divide-x sm:rtl:divide-x-reverse dark:divide-gray-600"
        >
            <div class="flex items-center space-x-1 sm:pe-4 rtl:space-x-reverse">
                <button class="btn" onclick={() => insert('**', '**')} id="format-strong">
                    <Bold />
                </button>
                <button class="btn" onclick={() => insert('*', '*')} id="format-em">
                    <Italic />
                </button>
                <button class="btn" onclick={insertURL} id="format-link">
                    <Link />
                </button>
                <button class="btn" onclick={() => insert('\n```\n', '\n```\n')} id="format-code">
                    <Code />
                </button>
                <div class="dropdown dropdown-bottom">
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
                <div class="dropdown dropdown-bottom">
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
        </div>
        <div class="fieldset-label">{textAreaContent.length}</div>
        <button
            type="button"
            data-tooltip-target="tooltip-fullscreen"
            class="cursor-pointer rounded p-2 text-gray-500 hover:bg-gray-100 hover:text-gray-900 sm:ms-auto dark:text-gray-400 dark:hover:bg-gray-600 dark:hover:text-white"
        >
            <svg
                class="h-4 w-4"
                aria-hidden="true"
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 19 19"
            >
                <path
                    stroke="currentColor"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M13 1h5m0 0v5m0-5-5 5M1.979 6V1H7m0 16.042H1.979V12M18 12v5.042h-5M13 12l5 5M2 1l5 5m0 6-5 5"
                />
            </svg>
            <span class="sr-only">Full screen</span>
        </button>
        <div
            id="tooltip-fullscreen"
            role="tooltip"
            class="tooltip invisible absolute z-10 inline-block rounded-lg bg-gray-900 px-3 py-2 text-sm font-medium text-white opacity-0 shadow-sm transition-opacity duration-300 dark:bg-gray-700"
        >
            Show full screen
            <div class="tooltip-arrow" data-popper-arrow></div>
        </div>
    </div>
    <div class="rounded-b-lg bg-white px-4 py-2 dark:bg-gray-800">
        <label for="editor" class="sr-only">Publish post</label>
        <textarea
            id="editor"
            bind:this={textArea}
            bind:value={textAreaContent}
            rows="8"
            class="textarea-xl block w-full border-0 bg-white px-0 text-sm text-gray-800 focus:ring-0 dark:bg-gray-800 dark:text-white dark:placeholder-gray-400"
            placeholder="Write an article..."
            required
        ></textarea>
    </div>
</div>
<button type="submit" class="btn"> Publish post </button>
