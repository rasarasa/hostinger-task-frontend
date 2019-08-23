<template>
    <component :is="tag"
               v-bind="$attrs"
               :type="(tag === 'button') ? ($attrs.type || 'button') : undefined"
               class="button"
               :class="{
                   [`button--${theme}`] : theme !== '',
                   [`button--${size}`] : size !== '',
               }"
    >
        <span class="button__inner">
            <slot />
        </span>
    </component>
</template>

<script>
export default {
    name: 'BaseButton',
    props: {
        tag: {
            type: String,
            default: 'button',
            validator: value => ['button', 'a'].includes(value),
        },
        theme: {
            type: String,
            default: 'primary',
            validator: value => ['', 'primary', 'secondary'].includes(value),
        },
        size: {
            type: String,
            default: 'md',
            validator: value => ['', 'md', 'lg'].includes(value),
        },
    },
};
</script>

<style scoped lang="scss">
.button {
    $_this: &;

    display: inline-flex;
    align-items: center;
    justify-content: center;
    padding: 0;
    border: 0;
    border-radius: 5px;
    background: transparent;
    font-family: $font-family-base;
    text-decoration: none;
    text-transform: uppercase;

    &__inner {
        position: relative;
        z-index: 1;
    }

    &--primary {
        @include hover-decoration(100%, $color-button-primary-hover);

        border-color: $color-button-primary;
        background: $color-button-primary;
        color: #fff;
    }

    &--secondary {
        @include hover-decoration(100%, $color-button-secondary-hover);

        border-color: $color-button-secondary;
        background: $color-button-secondary;
        color: #fff;
    }

    &--lg {
        padding: .9em 2.74em;
        font-size: 22px;
        font-weight: 600;
        line-height: 1.4em;
    }

    @include respond-to($media-md, max) {
        &--lg {
            font-size: 17px;
        }
    }
}
</style>
