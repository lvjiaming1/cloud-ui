<template>
<label :class="$style.root" :disabled="currentDisabled" @click="check()"
    tabindex="0" @keydown.space.prevent @keyup.space.prevent="check()"
    @focus="onFocus" @blur="onBlur" v-on="listeners">
    <span :class="$style.box" :status="String(currentValue)" :disabled="currentDisabled"></span>
    <slot></slot>
    <span vusion-slot-name="item">
        <slot name="item" :item="node">{{ text }}</slot>
        <s-empty v-if="!$slots.item && !text && $env.VUE_APP_DESIGNER && ($attrs['vusion-node-path'] || $attrs.designer)" inline :class="$style.empty"></s-empty>
    </span>
</label>
</template>

<script>
import { MChild } from '../m-parent.vue';
import MField from '../m-field.vue';
import SEmpty from '../s-empty.vue';

export default {
    name: 'u-checkbox',
    parentName: 'u-checkboxes',
    components: {
        SEmpty,
    },
    mixins: [MChild, MField],
    props: {
        value: { type: [String, Boolean], default: false },
        label: null,
        text: String,
        readonly: { type: Boolean, default: false },
        disabled: { type: Boolean, default: false },
        autofocus: { type: Boolean, default: false },
        node: Object,
    },
    data() {
        return { parentVM: undefined, currentValue: this.value };
    },
    computed: {
        listeners() {
            const listeners = Object.assign({}, this.$listeners);
            ['focus', 'blur', 'update:value'].forEach((prop) => {
                delete listeners[prop];
            });
            return listeners;
        },
        currentDisabled() {
            return this.disabled || (this.parentVM && this.parentVM.disabled) || (this.parentVM && this.parentVM.exceedMax() && !this.currentValue);
        },
    },
    watch: {
        value(value) {
            this.currentValue = value;
        },
        currentValue(value, oldValue) {
            this.$emit('change', { value, oldValue });
        },
    },
    mounted() {
        this.autofocus && this.$el.focus();
    },
    methods: {
        onFocus(e) {
            this.$emit('focus', e);
        },
        onBlur(e) {
            this.$emit('blur', e);
        },
        check() {
            if (this.readonly || this.disabled)
                return;
            const oldValue = this.currentValue;
            const value = !this.currentValue;
            if (
                this.parentVM
                && !this.parentVM.canCheck({
                    value,
                    oldValue,
                    label: this.label,
                    itemVM: this,
                })
            )
                return;
            let cancel = false;
            this.$emit('before-check', {
                value,
                oldValue,
                label: this.label,
                preventDefault: () => (cancel = true),
            });
            if (cancel)
                return;
            this.currentValue = value;
            this.$emit('input', value);
            this.$emit('update:value', value);
            this.$emit('check', { value, oldValue });
            this.parentVM
                && this.parentVM.onCheck({
                    value,
                    oldValue,
                    label: this.label,
                    itemVM: this,
                });
        },
    },
};
</script>

<style module>
.root {
    user-select: none;
    cursor: var(--cursor-pointer);
    color: var(--checkbox-font-color);
}

.root:focus {
    outline: var(--focus-outline);
}

.root:focus .box,
.root:focus .box[status="true"] {
    box-shadow: var(--checkbox-box-shadow-focus);
    border-color: var(--checkbox-border-color-focus);
}

.root:active {
    outline: var(--focus-outline);
}
.root:active .box,
.root:active .box[status="true"] {
    box-shadow: var(--checkbox-box-shadow-active);
    border-color: var(--checkbox-border-color-hover);
}

.root[disabled] {
    cursor: var(--cursor-not-allowed);
    color: var(--checkbox-font-color-disabled);
}

.root[disabled]:focus .box,
.root[disabled]:focus .box[status="true"]{
    box-shadow: none;
    border-color: var(--checkbox-border-color-disabled);
}

.root[disabled]:active .box,
.root[disabled]:active .box[status="true"]{
    box-shadow: none;
    border-color: var(--checkbox-border-color);
}

.box {
    position: relative;
    top: var(--checkbox-icon-top);
    display: inline-block;
    width: var(--checkbox-size);
    height: var(--checkbox-size);
    line-height: calc(var(--checkbox-size) - 2px);
    background: var(--checkbox-background-default);
    border: 1px solid var(--checkbox-border-color);
    border-radius: var(--checkbox-border-radius);
    margin-right: var(--checkbox-inner-space-x);
    text-align: center;
    font-size: var(--checkbox-icon-size);
    transition: all var(--transition-duration-base);
    vertical-align: 1px;
}

.box:hover {
    border-color: var(--checkbox-border-color-hover);
}

.box::before {
    transition: all var(--transition-duration-base);
}

.box[status="true"]::before, .box[status="false"]::before, .box[status="null"]::before {
    position: relative;
    display: block;
    left: var(--checkbox-icon-left);
    width: calc(var(--checkbox-size) - 2px);
    height: calc(var(--checkbox-size) - 2px);
}

.box[status="false"]::before {
    icon-font: url('./assets/check.svg') '\ff01';
    icon-font: url('./assets/check-dark.svg') '\ff02';
    content: var(--checkbox-icon);
    color: transparent;
}

.box[status="true"] {
    background: var(--checkbox-background);
    border-color: var(--checkbox-background);
}
.box[status="true"]::before {
    icon-font: url('./assets/check.svg') '\ff01';
    icon-font: url('./assets/check-dark.svg') '\ff02';
    content: var(--checkbox-icon);
    color: var(--checkbox-color);
}
.box[status="true"][disabled]::before {
    color: var(--checkbox-color-disabled);
}

.box[status="null"] {
    background: var(--checkbox-background);
    border-color: var(--checkbox-background);
}
.box[status="null"]::before {
    icon-font: url('./assets/check-null.svg');
    color: var(--checkbox-color);
    font-weight: bold;
}

.box[disabled] {
    border-color: var(--checkbox-border-color-disabled);
    background: var(--checkbox-background-disabled);
}

.root[designer]{
    display: inline-block;
    position: relative;
}
.root[designer] + .root[designer]:after{
    content: '';
    position: absolute;
    display: block;
    background: rgba(255,255,255,0.8);
    top: 0;
    left: 0;
    bottom: 0;
    right: 0;
}

.empty:not(:only-child){
    display: none;
}
</style>
