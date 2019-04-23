<template>
    <default-field :field="field" :errors="errors" :fullWidthContent="true">
        <template slot="field">

            <editor-menu-bar :editor="editor">
                <div class="menubar" slot-scope="{ commands, isActive }">
                    <div class="toolbar">
                        <button
                                class="menubar__button"
                                @click="commands.undo"
                        >
                            <icon name="undo" />
                        </button>

                        <button
                                class="menubar__button"
                                @click="commands.redo"
                        >
                            <icon name="redo" />
                        </button>

                        <button
                                class="menubar__button"
                                :class="{ 'is-active': isActive.bold() }"
                                @click="commands.bold"
                        >
                            <icon name="bold" />
                        </button>

                        <button
                                class="menubar__button"
                                :class="{ 'is-active': isActive.italic() }"
                                @click="commands.italic"
                        >
                            <icon name="italic" />
                        </button>

                        <button
                                class="menubar__button"
                                :class="{ 'is-active': isActive.strike() }"
                                @click="commands.strike"
                        >
                            <icon name="strike" />
                        </button>

                        <button
                                class="menubar__button"
                                :class="{ 'is-active': isActive.underline() }"
                                @click="commands.underline"
                        >
                            <icon name="underline" />
                        </button>

                        <button
                                class="menubar__button"
                                :class="{ 'is-active': isActive.code() }"
                                @click="commands.code"
                        >
                            <icon name="code" />
                        </button>

                        <button
                                class="menubar__button"
                                :class="{ 'is-active': isActive.paragraph() }"
                                @click="commands.paragraph"
                        >
                            <icon name="paragraph" />
                        </button>

                        <button
                                class="menubar__button"
                                :class="{ 'is-active': isActive.heading({ level: 1 }) }"
                                @click="commands.heading({ level: 1 })"
                        >
                            H1
                        </button>

                        <button
                                class="menubar__button"
                                :class="{ 'is-active': isActive.heading({ level: 2 }) }"
                                @click="commands.heading({ level: 2 })"
                        >
                            H2
                        </button>

                        <button
                                class="menubar__button"
                                :class="{ 'is-active': isActive.heading({ level: 3 }) }"
                                @click="commands.heading({ level: 3 })"
                        >
                            H3
                        </button>

                        <button
                                class="menubar__button"
                                :class="{ 'is-active': isActive.bullet_list() }"
                                @click="commands.bullet_list"
                        >
                            <icon name="ul" />
                        </button>

                        <button
                                class="menubar__button"
                                :class="{ 'is-active': isActive.ordered_list() }"
                                @click="commands.ordered_list"
                        >
                            <icon name="ol" />
                        </button>

                        <button
                                class="menubar__button"
                                :class="{ 'is-active': isActive.blockquote() }"
                                @click="commands.blockquote"
                        >
                            <icon name="quote" />
                        </button>

                        <button
                                class="menubar__button"
                                :class="{ 'is-active': isActive.code_block() }"
                                @click="commands.code_block"
                        >
                            <icon name="code" />
                        </button>

                        <button
                                class="menubar__button"
                                @click="commands.createTable({rowsCount: 3, colsCount: 3, withHeaderRow: false })"
                        >
                            <icon name="table" />
                        </button>

                        <span v-if="isActive.table()">
						<button
                                class="menubar__button"
                                @click="commands.deleteTable"
                        >
							<icon name="delete_table" />
						</button>
						<button
                                class="menubar__button"
                                @click="commands.addColumnBefore"
                        >
							<icon name="add_col_before" />
						</button>
						<button
                                class="menubar__button"
                                @click="commands.addColumnAfter"
                        >
							<icon name="add_col_after" />
						</button>
						<button
                                class="menubar__button"
                                @click="commands.deleteColumn"
                        >
							<icon name="delete_col" />
						</button>
						<button
                                class="menubar__button"
                                @click="commands.addRowBefore"
                        >
							<icon name="add_row_before" />
						</button>
						<button
                                class="menubar__button"
                                @click="commands.addRowAfter"
                        >
							<icon name="add_row_after" />
						</button>
						<button
                                class="menubar__button"
                                @click="commands.deleteRow"
                        >
							<icon name="delete_row" />
						</button>
						<button
                                class="menubar__button"
                                @click="commands.toggleCellMerge"
                        >
							<icon name="combine_cells" />
						</button>
					</span>
                    </div>
                </div>
            </editor-menu-bar>

            <editor-content
                class="
                tiptap-content
                py-3 h-auto
                pr-6
                pb-4
                pt-4
                w-full
                form-control
                form-input
                form-input-bordered
                mt-2
                no-focus
                "
                :editor="editor"
            />
        </template>
    </default-field>
</template>

<script>
import { FormField, HandlesValidationErrors } from 'laravel-nova'
import { Editor, EditorContent, EditorMenuBar, EditorMenuBubble } from 'tiptap'
import HeadingButtons from './buttons/HeadingButtons';
import NormalButton from './buttons/NormalButton';
import LinkButton from './buttons/LinkButton';

import {
    Blockquote,
    CodeBlock,
    HardBreak,
    Heading,
    OrderedList,
    BulletList,
    HorizontalRule,
    ListItem,
    TodoItem,
    TodoList,
    Bold,
    Code,
    Italic,
    Link,
    Strike,
    Underline,
    History,
    Table,
    TableHeader,
    TableCell,
    TableRow
} from 'tiptap-extensions'

export default {
    mixins: [FormField, HandlesValidationErrors],

    props: ['resourceName', 'resourceId', 'field'],

    components: {
        EditorContent,
        EditorMenuBar,
        HeadingButtons,
        NormalButton,
        LinkButton,
    },

    data: function () {
        return {
            headingLevels: 3,

            linkUrl: null,

            linkMenuIsActive: false,

            editor: new Editor({
                extensions: [
                    new Blockquote(),
                    new BulletList(),
                    new CodeBlock(),
                    new HardBreak(),
                    new Heading({ levels: [1, 2, 3] }),
                    new HorizontalRule(),
                    new ListItem(),
                    new OrderedList(),
                    new TodoItem(),
                    new TodoList(),
                    new Bold(),
                    new Code(),
                    new Italic(),
                    new Link(),
                    new Strike(),
                    new Underline(),
                    new History(),
                    new Table(),
                    new TableHeader(),
                    new TableCell(),
                    new TableRow(),
                ],
            }),
        }
    },

    computed: {
        buttons() {
            return this.field.buttons ? this.field.buttons : ['bold', 'italic'];
        }
    },

    methods: {
        initEditor() {
            this.editor.setContent(this.value);

            // set the value each time editor is updated
            let outsideScope = this;
            this.editor.setOptions({
                onUpdate: function (state) {
                    outsideScope.value = state.getHTML();
                }
            });

            // set heading levels
            if (this.field.headingLevels) {
                this.headingLevels = this.field.headingLevels;
            } else {
                // fallback for the old style like this: 'heading' => 4
                _.forEach(this.buttons, function(params, key) {
                    if (key == 'heading') {
                        this.headingLevels = params;
                    }
                }.bind(this));
            }

        },

        showLinkMenu(attrs) {
            this.linkUrl = attrs.href;
            this.linkMenuIsActive = true;
        },

        hideLinkMenu() {
            this.linkUrl = null;
            this.linkMenuIsActive = false;
        },

        setLinkUrl(command, url) {
            command({ href: url });
            this.hideLinkMenu();
            this.editor.focus();
        },
    },

    mounted: function () {
        this.initEditor();
    }
}
</script>

<style>
.tiptap-content {
    outline: none !important;
    box-shadow: none !important;
}

.ProseMirror:focus {
    outline: none;
}

.ProseMirror ins {
    text-decoration: none;
    background-color: highlight;
}

.ProseMirror p, ul, ol, blockquote, pre {
    margin-bottom: 1em;
}

.ProseMirror h1, .ProseMirror h2, .ProseMirror h3, .ProseMirror h4, .ProseMirror h5, .ProseMirror h6 {
    margin-bottom: 0.5em;
}

.ProseMirror pre {
    white-space: pre-line;
}

.ProseMirror blockquote {
    border-left: 4px solid var(--60);
    padding-left: 12px;
    margin-left: 0;
    font-style: italic;
}

.ProseMirror p:last-child {
    margin-bottom: 0;
}

.tiptap-button {
    box-sizing: border-box;
    vertical-align: top;
    margin-right: 8px;
    font-weight: 400;
}

.tiptap-button-container {
    position: relative;
    overflow: visible;
}

.tiptap-button-form {
    position: absolute;
    top: 36px;
    left: -130px;
    display: flex;
    background-color: white;
    align-items: center;
}

.tiptap-button-form .form-input {
    width: 180px;
    margin-right: 20px;
    color: black;
}

.tiptap-button-form .btn {
    width: 16px;
    height: 16px;
}


.tiptap-button::before {
    margin: 0;
    height: 16px;
    line-height: 0;
    position: relative;
}

.tiptap-button.is-bold::before {
    content: 'B';
}

.tiptap-button.is-italic::before {
    font-style: italic;
    content: 'I';
}

.tiptap-button.is-strike::before {
    text-decoration: line-through;
    content: 'S';
}

.tiptap-button.is-underline::before {
    text-decoration: underline;
    content: 'U';
}

.tiptap-button.is-code::before {
    content: '<>';
}

.tiptap-button.is-code_block::before {
    content: '</>';
}




</style>
