<template>
<div :class="$style.root">
    <div :class="$style.main" :unit="!!unit">
         <!-- @override: 增加提示 tip -->
        <div :class="$style.tip" vusion-slot-name="tip" vusion-slot-name-edit="tip">
            <slot name="tip">
                {{ tip }}
                <s-empty
                    v-if="(!$slots.tip)
                        && !tip
                        && $env.VUE_APP_DESIGNER
                        && !!$attrs['vusion-node-path']">
                </s-empty>
            </slot>
        </div>
        <u-slider :class="$style.slider" @mousedown.native="onMousedown" :value="currentValue" @input="onInput" @slide="onSlide"
            :min="min" :max="max" :step="step" :precision="precision" :range="range" :readonly="readonly" :disabled="disabled"
            :show-tooltip="showTooltip" :tooltip="tooltip" :placement="placement"
        ></u-slider>
         <!-- @override: 增加数值显示 -->
        <div :class="$style.scales">
            <span :class="$style.scale">{{ min }}{{ unit }}</span>
            <span :class="$style.scale">{{ ((min - 0) + (max - 0)) / 2 >> 0 }}{{ unit }}</span>
            <span :class="$style.scale">{{ max }}{{ unit }}</span>
        </div>
    </div>
    <u-number-input :class="$style.input" :value="currentValue" @change="onInput($event.value, 'numberInput')" @validate="onValidate"
        :min="numberMin" :max="numberMax" :step="step || 1" :precision="precision"
        :formatter="formatter" :hide-buttons="hideButtons"
        :readonly="readonly" :disabled="disabled"
    ></u-number-input>
    <slot>
        <!-- @override: 增加单位显示 -->
        <span>{{ unit }}</span>
    </slot>
</div>
</template>

<script>
import MField from '../m-field.vue';
import SEmpty from '../s-empty.vue';

export default {
    name: 'u-combo-slider',
    components: { SEmpty },
    mixins: [MField],
    props: {
        value: { type: Number, default: 0 },
        min: { type: Number, default: 0 },
        max: { type: Number, default: 100 },
        step: { type: Number, default: 1, validator: (step) => step >= 0 },
        precision: {
            type: Number,
            default: 1,
            validator: (precision) => precision > 0,
        },
        range: {
            type: Array,
            default() {
                return [];
            },
        },
        formatter: { type: [String, Object] },
        syncOn: { type: String, default: 'input' },
        hideButtons: { type: Boolean, default: true },
        unit: String,
        tip: String,
        readonly: { type: Boolean, default: false },
        disabled: { type: Boolean, default: false },
        showTooltip: { type: Boolean, default: false },
        tooltip: String,
        placement: { type: String, default: 'top' },
        layout: { type: String, default: 'flex' },
    },
    data() {
        return { currentValue: this.value, isMousedown: false };
    },
    computed: {
        numberMin() {
            return Math.max(
                this.min,
                this.range[0] === undefined ? -Infinity : this.range[0],
            );
        },
        numberMax() {
            return Math.min(
                this.max,
                this.range[1] === undefined ? Infinity : this.range[1],
            );
        },
    },
    watch: {
        value(value) {
            this.currentValue = value;
        },
        currentValue(currentValue, oldValue) {
            this.$emit('change', { value: currentValue, oldValue }, this);
        },
    },
    created() {
        document.body.addEventListener('mouseup', this.onMouseup, false);
    },
    destroyed() {
        document.body.removeEventListener('mouseup', this.onMouseup, false);
    },
    methods: {
        onValidate($event) {
            if (this.syncOn === 'blur')
                return;
            const value = $event.value; // 最小值的情况不同步，不然会显得很怪异
            if (!$event.valid && value === this.min)
                return;
            this.currentValue = value;
            this.$emit('input', value, this);
            this.$emit('update:value', value, this);
        },
        onInput(value, type) {
            this.currentValue = value;
            this.$emit('input', value, this);
            this.$emit('update:value', value, this);
            if (type) {
                // 区分onInput事件的来源 满足特定需求
                this.$emit('number-input', value);
            }
        },
        onSlide($event) {
            this.$emit('slide', $event, this);
        },
        onMousedown(e) {
            this.isMousedown = true;
        },
        onMouseup(e) {
            if (this.isMousedown) {
                this.$emit('slide-end', this.currentValue);
                this.isMousedown = false;
            }
        },
    },
};
</script>

<style module>
.root {
  text-align: center;
  display: flex;
  align-items: baseline;
}

.slider {
    display: inline-block;
    width: var(--combo-slider-slider-width);
}

.input {
    margin-left: var(--combo-slider-input-margin-left);
    width: var(--combo-slider-input-width) !important;
    margin-right: 0.5em;
    padding: 0 4px;
    height: 32px;
    line-height: 32px;
    text-align: center;
    background-color: var(--combo-slider-input-background) !important;;
    border-color: var(--combo-slider-input-border-color) !important;;
    border-radius: var(--combo-slider-input-border-radius) !important;;
    color: var(--combo-slider-input-color) !important;;
}

.main {
    display: inline-block;
    width: calc(100% - var(--combo-slider-input-width) - 20px);
    vertical-align: -20px;
}
.main[unit="true"] {
    width: calc(100% - var(--combo-slider-input-width) - 20px - var(--combo-slider-input-margin-left) - 0.5em);
}

.body {
    margin: 0 7px;
}

.tip {
    float: left;
    margin-left: 7px;
    margin-bottom: 4px;
    font-size: 12px;
    color: var(--combo-slider-tip-color);
    display: flex;
    flex-wrap: wrap;
    align-items: var(--combo-slider-tip-align-items);
}

.scales {
    margin: 0 7px;
    font-size: 12px;
    color: var(--combo-slider-scales-color);
}

.scale {
    display: inline-block;
    width: 33.333333%;
    text-align: center;
}

.scale:first-child {
    text-align: left;
}

.scale:last-child {
    text-align: right;
}
</style>
