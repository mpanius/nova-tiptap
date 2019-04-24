<template>
    <default-field :field="field" :errors="errors" :fullWidthContent="true">
        <template slot="field">
            <Modal ref="ytmodal" @onConfirm="addCommand" />
            <editor-menu-bar :editor="editor">

                <div class="menubar" slot-scope="{ commands, isActive }">
                    <div class="toolbar">
                        <button
                                type="button"
                                class="menubar__button"
                                @click="commands.undo"
                        >
                            <undo-icon></undo-icon>
                        </button>

                        <button
                                type="button"
                                class="menubar__button"
                                @click="commands.redo"
                        >
                            <redo-icon></redo-icon>
                        </button>

                        <button
                                type="button"
                                class="menubar__button"
                                :class="{ 'is-active': isActive.bold() }"
                                @click="commands.bold"
                        >
                            <format-bold-icon></format-bold-icon>
                        </button>

                        <button
                                type="button"
                                class="menubar__button"
                                :class="{ 'is-active': isActive.italic() }"
                                @click="commands.italic"
                        >
                            <format-italic-icon></format-italic-icon>
                        </button>

                        <button
                                type="button"
                                class="menubar__button"
                                :class="{ 'is-active': isActive.strike() }"
                                @click="commands.strike"
                        >
                            <format-strikethrough-icon></format-strikethrough-icon>
                        </button>

                        <button
                                type="button"
                                class="menubar__button"
                                :class="{ 'is-active': isActive.underline() }"
                                @click="commands.underline"
                        >
                            <format-underline-icon></format-underline-icon>
                        </button>

                        <button
                                type="button"
                                class="menubar__button"
                                :class="{ 'is-active': isActive.code() }"
                                @click="commands.code"
                        >
                            <code-braces-icon></code-braces-icon>
                        </button>

                        <button
                                type="button"
                                class="menubar__button"
                                :class="{ 'is-active': isActive.paragraph() }"
                                @click="commands.paragraph"
                        >
                            <format-paragraph-icon></format-paragraph-icon>
                        </button>

                        <button
                                type="button"
                                class="menubar__button"
                                :class="{ 'is-active': isActive.heading({ level: 1 }) }"
                                @click="commands.heading({ level: 1 })"
                        >
                            <format-header1-icon></format-header1-icon>
                        </button>

                        <button
                                type="button"
                                class="menubar__button"
                                :class="{ 'is-active': isActive.heading({ level: 2 }) }"
                                @click="commands.heading({ level: 2 })"
                        >
                            <format-header2-icon></format-header2-icon>
                        </button>

                        <button
                                type="button"
                                class="menubar__button"
                                :class="{ 'is-active': isActive.heading({ level: 3 }) }"
                                @click="commands.heading({ level: 3 })"
                        >
                            <format-header3-icon></format-header3-icon>
                        </button>

                        <button
                                type="button"
                                class="menubar__button"
                                :class="{ 'is-active': isActive.bullet_list() }"
                                @click="commands.bullet_list"
                        >
                            <format-underline-icon></format-underline-icon>
                        </button>

                        <button
                                type="button"
                                class="menubar__button"
                                :class="{ 'is-active': isActive.ordered_list() }"
                                @click="commands.ordered_list"
                        >
                            <format-list-bulleted-icon></format-list-bulleted-icon>
                        </button>

                        <button
                                type="button"
                                class="menubar__button"
                                :class="{ 'is-active': isActive.blockquote() }"
                                @click="commands.blockquote"
                        >
                           <format-quote-open-icon></format-quote-open-icon>
                        </button>

                        <button
                                type="button"
                                class="menubar__button"
                                :class="{ 'is-active': isActive.code_block() }"
                                @click="commands.code_block"
                        >
                            <code-brackets-icon></code-brackets-icon>
                        </button>

                        <button
                                type="button"
                                class="menubar__button"
                                @click="commands.createTable({rowsCount: 2, colsCount: 2, withHeaderRow: true })"
                        >
                            <table-icon></table-icon>
                        </button>

                        <span v-if="isActive.table()">
						<button
                                type="button"
                                class="menubar__button"
                                @click="commands.deleteTable"
                        >
							<table-remove-icon></table-remove-icon>
						</button>
						<button
                                type="button"
                                class="menubar__button"
                                @click="commands.addColumnBefore"
                        >
							<table-column-plus-before-icon></table-column-plus-before-icon>
						</button>
						<button
                                type="button"
                                class="menubar__button"
                                @click="commands.addColumnAfter"
                        >
							<table-column-plus-after-icon></table-column-plus-after-icon>
						</button>
						<button
                                type="button"
                                class="menubar__button"
                                @click="commands.deleteColumn"
                        >
							<table-column-remove-icon></table-column-remove-icon>
						</button>
						<button
                                type="button"
                                class="menubar__button"
                                @click="commands.addRowBefore"
                        >
							<table-row-plus-before-icon></table-row-plus-before-icon>
						</button>
						<button
                                type="button"
                                class="menubar__button"
                                @click="commands.addRowAfter"
                        >
							<table-row-plus-after-icon></table-row-plus-after-icon>
						</button>
						<button
                                type="button"
                                class="menubar__button"
                                @click="commands.deleteRow"
                        >
							<table-row-remove-icon></table-row-remove-icon>
						</button>
						<button
                                type="button"
                                class="menubar__button"
                                @click="commands.toggleCellMerge"
                        >
							<table-merge-cells-icon></table-merge-cells-icon>
						</button>
					</span>
                        <button
                                type="button"
                                class="menubar__button"
                                @click="openModal(commands.image);">
                            <image-icon></image-icon>
                        </button>
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
import Modal from "./modal/modal"


import {
    Blockquote,
    CodeBlock,
    HardBreak,
    Heading,
    OrderedList,
    BulletList,
    ListItem,
    TodoItem,
    TodoList,
    Bold,
    Code,
    Italic,
    Link,
    Table,
    TableHeader,
    TableCell,
    TableRow,
    Strike,
    Underline,
    History,
    Image,
} from 'tiptap-extensions'
import UndoIcon from "vue-material-design-icons/Undo";
import RedoIcon from "vue-material-design-icons/Redo";
import FormatBoldIcon from "vue-material-design-icons/FormatBold";
import FormatItalicIcon from "vue-material-design-icons/FormatItalic";
import FormatStrikethroughIcon from "vue-material-design-icons/FormatStrikethrough";
import FormatUnderlineIcon from "vue-material-design-icons/FormatUnderline";
import CodeBracesIcon from "vue-material-design-icons/CodeBraces";
import FormatParagraphIcon from "vue-material-design-icons/FormatParagraph";
import FormatListBulletedIcon from "vue-material-design-icons/FormatListBulleted";
import FormatQuoteOpenIcon from "vue-material-design-icons/FormatQuoteOpen";
import CodeBracketsIcon from "vue-material-design-icons/CodeBrackets";
import TableIcon from "vue-material-design-icons/Table";
import TableRemoveIcon from "vue-material-design-icons/TableRemove";
import TableColumnPlusBeforeIcon from "vue-material-design-icons/TableColumnPlusBefore";
import TableColumnPlusAfterIcon from "vue-material-design-icons/TableColumnPlusAfter";
import TableColumnRemoveIcon from "vue-material-design-icons/TableColumnRemove";
import TableRowPlusAfterIcon from "vue-material-design-icons/TableRowPlusAfter";
import TableRowRemoveIcon from "vue-material-design-icons/TableRowRemove";
import TableMergeCellsIcon from "vue-material-design-icons/TableMergeCells";
import FormatHeader1Icon from "vue-material-design-icons/FormatHeader1";
import FormatHeader2Icon from "vue-material-design-icons/FormatHeader2";
import FormatHeader3Icon from "vue-material-design-icons/FormatHeader3";
import ImageIcon from "vue-material-design-icons/Image";

export default {
    mixins: [FormField, HandlesValidationErrors],

    props: ['resourceName', 'resourceId', 'field'],

    components: {
        ImageIcon,
        FormatHeader3Icon,
        FormatHeader2Icon,
        FormatHeader1Icon,
        TableMergeCellsIcon,
        TableRowRemoveIcon,
        TableRowPlusAfterIcon,
        TableColumnRemoveIcon,
        TableColumnPlusAfterIcon,
        TableColumnPlusBeforeIcon,
        TableRemoveIcon,
        TableIcon,
        CodeBracketsIcon,
        FormatQuoteOpenIcon,
        FormatListBulletedIcon,
        FormatParagraphIcon,
        CodeBracesIcon,
        FormatUnderlineIcon,
        FormatStrikethroughIcon,
        FormatItalicIcon,
        FormatBoldIcon,
        RedoIcon,
        UndoIcon,
        EditorContent,
        EditorMenuBar,
        Modal,
    },

    data: function () {
        return {
            headingLevels: 3,

            linkUrl: true,

            linkMenuIsActive: true,

            editor: new Editor({
                extensions: [
                    new Blockquote(),
                    new BulletList(),
                    new CodeBlock(),
                    new HardBreak(),
                    new Heading({ levels: [1, 2, 3] }),
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
                    new Image(),
                ],
            }),
        }
    },


    methods: {
        openModal(command) {
            this.$refs.ytmodal.showModal(command);
        },
        addCommand(data) {
            if (data.command !== null) {
                data.command(data.data);
            }
        },
        initEditor() {
            try {
                this.value = JSON.parse(this.value);
            } catch (e) {};
            this.editor.setContent(this.value);

            // set the value each time editor is updated
            let outsideScope = this;
            this.editor.setOptions({
                onUpdate: function (state) {
                    console.log(state.getJSON());
                    outsideScope.value = JSON.stringify(state.getJSON());
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

.ProseMirror .tableWrapper {
    margin: 1em 0;
    overflow-x: auto;
}
.ProseMirror table {
    border-collapse: collapse;
    table-layout: fixed;
    width: 100%;
    margin: 0;
    overflow: hidden;
}
.ProseMirror table th {
    font-weight: 700;
    text-align: left;
}
.ProseMirror table td,
.ProseMirror table th {
    min-width: 1em;
    border: 2px solid #ddd;
    padding: 3px 5px;
    vertical-align: top;
    -webkit-box-sizing: border-box;
    box-sizing: border-box;
    position: relative;
}

.menubar__button {
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


.menubar__button::before {
    margin: 0;
    height: 16px;
    line-height: 0;
    position: relative;
}



</style>
