<template>
    <nav>
        <ul>
            <li>Todoly</li>
            <li><button @click="expandForm" v-if="isCollapsed">Add new Todo</button></li>
            <li><button @click="emptyLocalStorage">Clear list</button></li>
        </ul>
    </nav>
</template>

<script>
    export default {
        props: ['isCollapsed'],

        methods: {
            emptyLocalStorage() {
                this.$emit('clearStorage')
            }, 
            
            expandForm(){
                this.$emit('open')
            }
        }
    }
</script>

<style scoped lang="scss">
    @import '../globals.scss';

    nav{
        width: 100dvw;

        ul{
            @include flex(row, center, space-between, 0);
            padding: var(--size-fluid-3);

            li:first-of-type{
                color: #fff;
                font-size: var(--font-size-fluid-2);
            }

            li{
                button{
                    all: unset;
                    background-color: coral;
                    border-radius: var(--radius-round);
                    padding: 12px;
                    // border-radius: 7px;
                    cursor: pointer;
                    font-weight: bold;

                    &:hover{
                        background-color: rgb(253, 112, 61);
                    }
                }
            }
        }
    }
</style>