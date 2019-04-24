<template>
    <span>
        <form
            class="
                tiptap-button-form
                btn
                btn-default
                p-2
            "
            v-show="linkMenuIsActive"
            @submit.prevent="setLinkUrl(commands.link, linkUrl)"
        >
            <input
                class="
                    form-input
                    form-input-bordered
                    p-1
                    text-xs
                    leading-none
                "
                type="text"
                v-model="linkUrl"
                placeholder="https://"
                ref="linkInput"
                @keydown.esc="hideLinkMenu"
            />

            <button
                class="btn is-close"
                @click="setLinkUrl(commands.link, null)"
                type="button"
            >
             <close-icon></close-icon>
            </button>
        </form>

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
                (isActive.link() ? 'btn-primary' : '')
            ]"
            @click="showLinkMenu(getMarkAttrs('link'))"
        >
            <link-icon></link-icon>
        </button>

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
                (isActive.link() ? 'btn-primary' : '')
            ]"
            @click="setLinkUrl(commands.link, null)"
        >
            <link-off-icon></link-off-icon>
        </button>
    </span>
</template>

<script>
    import MenuIcon from "vue-material-design-icons/Menu.vue"
    import LinkOffIcon from "vue-material-design-icons/LinkOff";
    import LinkBoxIcon from "vue-material-design-icons/LinkBox";
    import CloseIcon from "vue-material-design-icons/Close";
    import LinkIcon from "vue-material-design-icons/Link";


export default {
    props: [
        'getMarkAttrs',
        'linkMenuIsActive',
        'isActive',
        'commands',
        'linkUrl',
        'hideLinkMenu',
        'showLinkMenu',
        'setLinkUrl',
    ],

    components: {
        LinkIcon,
        CloseIcon,
        LinkOffIcon,
        LinkBoxIcon,
        MenuIcon,
    },
}
</script>



