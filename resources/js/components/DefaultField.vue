<template>
    <div ref="field-wrapper">
        <field-wrapper>
            <div :class="fieldWrapperClasses">

                <div :class="fieldLabelClasses">
                    <slot>
                        <form-label
                        :label-for="field.attribute"
                        :class="{ 'mb-2': showHelpText && field.helpText }"
                        >
                            {{ fieldLabel }}
                            <div v-if="showHelpText && field.helpText" class="help-text-tooltip-container">
                                <div class="help-text-tooltip">
                                    ?
                                </div>
                                <div class="help-text-tooltip-hidden">
                                    {{ field.helpText }}
                                </div>
                            </div>
                            <!-- <help-text class="help-text mt-2" v-if="showHelpText"> {{ field.helpText }} </help-text> -->
                        </form-label>
                    </slot>
                </div>
                <div :class="fieldClasses">
                    <slot name="field" />

                    <help-text class="error-text mt-2 text-danger" v-if="showErrors && hasError">
                        {{ firstError }}
                    </help-text>

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

        data:() => ({
            ready: false,
        }),

        mounted() {
         if (this.hasSize) {
            this.$parent.$parent.$el.classList.add('nova-grid-card-styles');
            const $fieldRefs = this.$refs;
            this.field.size.split(' ').forEach(function(singleClass)
            {
                $fieldRefs['field-wrapper'].classList.add(singleClass);
            });
        }
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

        hasSize() {
            return this.field.size !== undefined;
        },

        fieldLabelClasses() {
            return this.hasSize ? 'nova-grid-field-label' : 'w-1/5 py-6 px-8';
        },

        fieldWrapperClasses() {
            return this.field.size !== undefined ? 'nova-grid-wrapper' : 'flex border-b border-40 w-full';
        },

        fieldClasses() {

            if(this.hasSize) {
                return 'w-full';
            }

            return this.fullWidthContent ? 'py-6 px-8 w-4/5' : 'py-6 px-8 w-1/2'
        },
    },
}
</script>
<style lang="scss">

.nova-grid-wrapper {
    display: flex;
    flex-wrap: wrap;
    width: 100%;
    padding: 1rem;

}

.nova-grid-field-label {
    font-weight: 700;
    margin-bottom: 0.5rem;
    width: 100%;
}

.nova-grid-card-styles {

        display: flex;
        flex-wrap: wrap;
        width: 100%;

        .bg-30.flex.px-8.py-4 {
            width: 100%;
            display: flex;
            justify-content: flex-end;
        }
}

.help-text-tooltip-container {
    display: inline-block;
}
.help-text-tooltip {
    cursor: pointer;
    padding: 0 .4rem;
    border: 1px solid #ccc;
    border-radius: 20rem;
    background: #ffa;
}
.help-text-tooltip:hover ~ .help-text-tooltip-hidden {
    display: block;
}
.help-text-tooltip-hidden {
    display: none;
    position: absolute;
    padding: 1rem;
    background: #fff;
    border: 1px solid #999;
    border-radius: 0.5rem;
    z-index: 100;
    max-width: 300px;
}


// .help-text-tooltip {
//     display: inline-block;
//     cursor: pointer;
//     padding: 0 .4rem;
//     border: 1px solid #ccc;
//     border-radius: 20rem;
//     background: #ffa;
// }
// .help-text-tooltip:hover ~ .help-text-tooltip-hidden {
//     max-height: inherit;
//     // transform: translate(0, 0%);
// }
// .help-text-tooltip-hidden {
//     max-height: 0;
//     // transform: translate(0, -100%);
//     overflow: hidden;
//     transition: all .3s ease;
//     // transition: transform .3s ease;
// }

.w-fit-size-gallery {
    min-width: 356px;
    flex: 1;
    overflow: auto;
}
@media (max-width: 1580px) {
    .w-fit-size-gallery {
        width: 100%;
        flex: auto;
    }
}

</style>