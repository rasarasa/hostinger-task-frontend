<template>
    <layout-primary>
        <page-width>

            <transition name="fade">
                <div v-if="clickedCellData !== ''"
                     class="sticky-box"
                     :class="{ 'is-visible' : clickedCellData !== '' }"
                >
                    {{ clickedCellData }}
                </div>
            </transition>

            <div class="table-wrapper">
                <table class="table"
                       @click="(e) => { tableClickHandler(e); }"
                       @keyup.enter="(e) => { tableClickHandler(e); }"
                >
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
                            class="table__body-row"
                            :class="{ 'is-active' : (rowIndex === activeRow) }"
                            tabindex="0"
                        >
                            <td v-for="(col, colKey) in row"
                                :key="colKey"
                                :data-colkey="colKey"
                                :data-rowid="rowIndex"
                                v-if="!colKey.startsWith('_')"
                                class="table__body-cell"
                                tabindex="0"
                            >
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
            activeRow: null,
            clickedCellData: '',
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
        },
    },
    methods: {
        getItemValue(rowId, colKey) {
            return this.itemsSorted[rowId][colKey];
        },
        setSort(key) {
            if ( this.sortBy !== key ) {
                this.sortBy = key;
            } else {
                this.sortOrderDesc = !this.sortOrderDesc;
            }
        },
        tableClickHandler(e) {
            const col = e.target;
            const colKey = col.dataset.colkey;
            const rowId = +col.dataset.rowid; // "+" converts to number

            if (col.nodeName === "TD") { // check if not keypress on a row
                this.clickedCellData = this.getItemValue(rowId, colKey);
            }

            this.activeRow = rowId;
        }
    }
}
</script>

<style scoped lang="scss">
.fade-enter-active,
.fade-leave-active {
    @include transition(opacity);
}

.fade-enter,
.fade-leave-to {
    opacity: 0;
}

.sticky-box {
    $_width: 250px;

    position: fixed;
    z-index: z-index('page-header');
    top: $page-header-height;
    left: 50%;
    width: $_width;
    margin-left: -$_width/2;
    padding: .5em 1em;
    background-color: $color-highlight-red;
    color: #fff;
    text-align: center;
}

.table {
    $_this: &;
    $_spacing: 1em;

    width: 100%;
    margin: 42px 0 0;

    &-wrapper {
        margin: $_spacing (-$_spacing/2);
    }

    &__body-cell {
        padding: $_spacing/2 $_spacing;

        &:first-child {
            padding-left: $_spacing/2;
        }

        &:last-child {
            padding-right: $_spacing/2;
        }
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

        &:first-child {
            padding-left: 0;

            &::after {
                left: 0;
            }
        }

        &:last-child {
            padding-right: 0;

            &::after {
                right: 0;
            }
        }
    }

    &__sort-button {
        display: flex;
        z-index: 1;
        align-items: center;
        width: 100%;
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

    &__body-row {
        &.is-active {
            background-color: $color-bg-light;
        }
    }
}
</style>
