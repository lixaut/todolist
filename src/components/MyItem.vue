<template>
    <li>
        <label>
            <input type="checkbox" :checked="todoObj.done" @change="handleCheck"/>
            <span>{{todoObj.title}}</span>
        </label>
        <button class="btn btn-danger" @click="handleDelete(todoObj.id)">删除</button>
    </li>
</template>

<script>
    export default {
        name: 'MyItem',
        props: ['todoObj', 'checkTodo', 'deleteTodo'],
        methods: {
            handleCheck() {
                this.checkTodo(this.todoObj.id)
            },
            handleDelete(id) {
                if(confirm('确定删除该项目？'))
                this.deleteTodo(id)
            }
        }
    }
</script>

<style scoped>
    /* item */
    li {
        list-style: none;
        height: 36px;
        line-height: 36px;
        padding: 0 5px;
        border-bottom: 1px solid #ddd;
    }

    li label {
        float: left;
        cursor: pointer;
    }

    li label li input {
        vertical-align: middle;
        margin-right: 6px;
        position: relative;
        top: -1px;
    }

    li button {
        float: right;
        display: none;
        margin-top: 3px;
    }

    li:hover button {
        display: block;
    }

    li:before {
        content: initial;
    }

    li:last-child {
        border-bottom: none;
    }

    li:hover {
        background-color: #eee;
    }
</style>