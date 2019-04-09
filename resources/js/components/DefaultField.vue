<template>
    <div :class="field.size || 'w-full'">
        <field-wrapper >
            <div class="flex flex-wrap w-full p-4">
                <div class="font-bold mb-2 w-full">
                    <slot>
                        <form-label
                        :label-for="field.attribute"
                        :class="{ 'mb-2': showHelpText && field.helpText }"
                        >
                        {{ fieldLabel }}
                    </form-label>
                </slot>
            </div>
            <div  :class="fieldClasses">
                <slot name="field" />

                <help-text class="error-text mt-2 text-danger" v-if="showErrors && hasError">
                    {{ firstError }}
                </help-text>

                <help-text class="help-text mt-2" v-if="showHelpText"> {{ field.helpText }} </help-text>
            </div>

        </div>
    </field-wrapper>
</div>
</template>

<script>
    import { HandlesValidationErrors, Errors } from 'laravel-nova'

    export default {
        mixins: [HandlesValidationErrors],

        props: {
            field: { type: Object, required: true },
            fieldName: { type: String },
            showHelpText: { type: Boolean, default: true },
            showErrors: { type: Boolean, default: true },
            fullWidthContent: { type: Boolean, default: false },
        },

        mounted() {
            let element = this.$parent.$el.parentElement;
            let parent = element.parentNode;
            while (element.firstChild) parent.insertBefore(element.firstChild, element);
            parent.removeChild(element);
        },


        computed: {
            fieldLabel() {
            // If the field name is purposefully an empty string, then
            // let's show it as such
            if (this.fieldName === '') {
                return ''
            }

            return this.fieldName || this.field.singularLabel || this.field.name
        },

        fieldClasses() {
            return 'w-full';
        },
    },
}
</script>
