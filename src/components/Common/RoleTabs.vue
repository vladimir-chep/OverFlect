<template>
    <ul class="roles">
        <li v-for="(role, index, key) in roles" :key="key" class="roles__item">
            <a
                :class="['roleBtn', { 'is-active': currentRole === role }]"
                @click="switchList(role)"
            >
                <svg
                    class="roleBtn__icon"
                    viewBox="0 0 24 24"
                    fill="none"
                    xmlns="http://www.w3.org/2000/svg"
                    preserveAspectRatio="none"
                >
                    <component :is="role"></component>
                </svg>
                {{ role }}
            </a>
        </li>
    </ul>
</template>

<script>
import { mapGetters } from 'vuex';

export default {
    name: 'RoleTabs',
    data() {
        return {
            roles: ['tank', 'damage', 'support'],
        };
    },
    components: {
        Tank: () => import('@/components/Symbols/Role/Tank.vue'),
        Damage: () => import('@/components/Symbols/Role/Damage.vue'),
        Support: () => import('@/components/Symbols/Role/Support.vue'),
    },
    computed: {
        ...mapGetters('progress', ['currentRole']),
    },
    methods: {
        switchList(role) {
            this.$store.dispatch('progress/switchRole', role);
        },
    },
};
</script>
<style lang="scss" scoped>
@import '~@/styles/setup/variables';
@import '~@/styles/setup/mixin';

.roles {
    display: flex;
    align-items: center;
    text-align: center;

    &__item {
        flex: 1 1 0;
    }
}

.roleBtn {
    @include transition-default('background-color');
    display: block;
    position: relative;
    align-items: center;
    padding: 14px 0;
    color: $text-light;
    font-weight: bold;
    font-size: 1.6rem;
    text-transform: capitalize;
    cursor: pointer;

    &:after {
        position: absolute;
        bottom: 0;
        left: 0;
        width: 100%;
        height: 2px;
        border-radius: 4px;
        background: $hr-color;
        content: '';
    }

    &:hover {
        background: rgba($theme-color, .05);
    }

    svg {
        width: 12px;
        margin-right: 4px;

        g {
            @include transition-default('fill');
            fill: $text-light;
        }
    }

    &.is-active {
        color: $theme-color;

        &:after {
            background: $theme-color;
        }

        svg g {
            fill: $theme-color;
        }
    }
}

</style>