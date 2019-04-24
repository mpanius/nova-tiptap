<template>
    <button
        type="button"
        class="
            btn
            btn-default
            p-2
            leading-none
            text-xs
            min-w-8
            h-8
            tiptap-button
        "
        :class="[
            (buttonKey == 'bold' && isActive.bold() ? 'btn-primary' : ''),
            (buttonKey == 'italic' && isActive.italic() ? 'btn-primary' : ''),
            (buttonKey == 'code' && isActive.code() ? 'btn-primary' : ''),
            (buttonKey == 'bullet_list' && isActive.bullet_list() ? 'btn-primary' : ''),
            (buttonKey == 'ordered_list' && isActive.ordered_list() ? 'btn-primary' : ''),
            (buttonKey == 'code_block' && isActive.code_block() ? 'btn-primary' : ''),
            (buttonKey == 'blockquote' && isActive.blockquote() ? 'btn-primary' : ''),
            'is-' + buttonKey

        ]"
        @click="commands[buttonKey]"
    >
        <format-list-bulleted-icon v-if="buttonKey == 'bullet_list'" title="Ненумерованный список"></format-list-bulleted-icon>

        <format-list-numbered-icon v-if="buttonKey == 'ordered_list'" title="Нумерованный список">
        </format-list-numbered-icon>

        <format-quote-close-icon v-if="buttonKey == 'blockquote'" title="Цитата">
        </format-quote-close-icon>
    </button>
</template>

<script>
import MenuIcon from "vue-material-design-icons/Menu.vue"
import AndroidIcon from "vue-material-design-icons/Android";
import FormatListBulletedIcon from "vue-material-design-icons/FormatListBulleted";
import FormatListNumberedIcon from "vue-material-design-icons/FormatListNumbered";
import FormatQuoteCloseIcon from "vue-material-design-icons/FormatQuoteClose";
import FormatBoldIcon from "vue-material-design-icons/FormatBold";
import VisualStudioCodeIcon from "vue-material-design-icons/VisualStudioCode";



export default {
    props: ['buttonKey', 'isActive', 'commands'],

    components: {
        VisualStudioCodeIcon,
        FormatBoldIcon,
        FormatQuoteCloseIcon,
        FormatListNumberedIcon,
        FormatListBulletedIcon,
        AndroidIcon,
        MenuIcon,
    },
}
</script>



