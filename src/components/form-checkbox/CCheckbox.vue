<template>

    <div class="c-checkbox"
         :class="checked ? 'is-active' : ''">

        <label ref="label"
               class="c-checkbox__label"
               tabindex="0">

            <input type="checkbox"
                   :checked="checked"
                   :value="value"
                   :disabled="disabled"
                   @change="handleChange">

            <span class="c-checkbox__toggle">
                <span class="c-checkbox__toggle-mark"></span>
                <span class="c-checkbox__toggle-box"></span>
                <span class="c-checkbox__toggle-shadow"></span>
            </span>


            <span v-if="$slots.default"
                  class="c-checkbox__content">
                <slot></slot>
            </span>

        </label>

    </div>

</template>

<script>
    import {
        isArray,
        isBoolean
    } from '../../utils/helpers';

    export default {
        name: "c-checkbox",
        model: {
            prop: 'model',
            event: 'change'
        },
        props: {
            model: null,
            value: null,
            name: String,
            disabled: {
                type: Boolean,
                default: false
            }
        },
        computed: {
            checked() {
                if (isArray(this.model)) {
                    return this.model.includes(this.value);
                } else if (isBoolean(this.model)) {
                    return this.model;
                } else {
                    return this.model == this.value;
                }
            }
        },
        methods: {
            handleChange({target: {checked}}) {
                let isChecked = checked;
                let value = this.value;
                let newValue = value;

                if (isArray(this.model)) {
                    newValue = [...this.model];

                    if (isChecked) {
                        newValue.push(value);
                    } else {
                        newValue.splice(newValue.indexOf(value), 1);
                    }

                    this.$emit('change', newValue);
                } else if (isBoolean(this.model)) {
                    this.$emit('change', isChecked);
                } else {
                    this.$emit('change', value);
                }
            },
            focus() {
                this.$refs.label.focus();
            }
        }
    }
</script>
