<template>
    <layout-primary>
        <page-width>

            <div class="table-wrapper">
                <table class="table">
                    <thead>
                        <tr>
                            <th v-for="(heading, headingIndex) in headings"
                                :key="headingIndex"
                                class="table__header-cell"
                            >
                                <button class="table__sort-button"
                                        @click="setSort(heading)"
                                >
                                    <span>
                                        {{ heading }}
                                    </span>
                                    <svg class="table__sort-icon"
                                         :class="{
                                             'is-asc':  heading === sortBy && !sortOrderDesc,
                                             'is-desc': heading === sortBy && sortOrderDesc
                                         }"
                                    >
                                        <use xlink:href="#sort" />
                                    </svg>
                                </button>
                            </th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(row, rowIndex) in itemsSorted"
                            :key="rowIndex"
                        >
                            <td v-for="(col, colIndex) in row"
                                :key="colIndex"
                                v-if="!colIndex.startsWith('_')"
                                class="table__body-cell"
\                            >
                                {{ col }}
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>

        </page-width>
    </layout-primary>
</template>

<script>
import '@/assets/icons/sort.svg';
import axios from 'axios/index';
import data from "@/views/dataTable/data.json";
import LayoutPrimary from "@/components/pageLayout/LayoutPrimary";
import PageWidth from "@/components/pageLayout/PageWidth";

export default {
    name: 'ViewDataTable',
    components: {
        PageWidth,
        LayoutPrimary,
    },
    data() {
        return {
            itemsRaw: data.data,
            sortBy: 'age',
            sortOrderDesc: false,
        }
    },
    computed: {
        headings() {
            return Object.keys(this.itemsRaw[0]).filter(function(key) {
                return !key.startsWith('_');
            });
        },
        itemsSorted() {
            let items = this.itemsRaw;

            if (this.sortBy !== '') {
                items.sort((a, b) => {
                    let order = 1;
                    order = this.sortOrderDesc ? order * (-1) : order;

                    return (a[this.sortBy] > b[this.sortBy]) ? order : order * (-1);
                });
            }

            return items;
        }
    },
    methods: {
        setSort(key) {
            if ( this.sortBy !== key ) {
                this.sortBy = key;
            } else {
                this.sortOrderDesc = !this.sortOrderDesc;
            }
        },
    }
}
</script>

<style scoped lang="scss">
.table {
    $_this: &;
    $_spacing: 1em;

    width: 100%;

    &-wrapper {
        margin: $_spacing (-$_spacing);
    }

    &__body-cell {
        padding: $_spacing/2 $_spacing;
    }

    &__header-cell {
        position: relative;
        padding: 0 $_spacing/2;
        padding-bottom: $_spacing*.75;

        &::after {
            content: '';
            position: absolute;
            right: $_spacing/2;
            bottom: $_spacing/2;
            left: $_spacing/2;
            height: 2px;
            background-color: $color-bg-dark;
        }
    }

    &__sort-button {
        display: flex;
        z-index: 1;
        align-items: center;
        padding: $_spacing/2;
        border: 0;
        background-color: transparent;
        font-weight: 700;
        text-align: left;
        text-transform: capitalize;
        cursor: pointer;

        &:hover,
        &:focus {
            color: $color-link-hover;

            #{$_this}__sort-icon {
                &.is-asc {
                    color: $color-link-hover;
                }

                &.is-desc {
                    fill: $color-link-hover;
                }
            }
        }
    }

    &__sort-icon {
        flex-shrink: 0;
        width: 9px;
        height: 14px;
        margin-left: 5px;
        fill: $color-text-body;
        color: $color-text-body;

        &.is-asc {
            color: $color-link;
        }

        &.is-desc {
            fill: $color-link;
        }
    }
}
</style>