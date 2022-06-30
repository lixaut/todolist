<template>
    <li>
        <label>
            <input type="checkbox" :checked="todoObj.done" @change="handleCheck"/>
            <span v-show="!todoObj.isEdit">{{todoObj.title}}</span>
            <input 
                v-show="todoObj.isEdit" 
                type="text" 
                :value="todoObj.title"
                @blur="handleBlur(todoObj, $event)"
                ref="inputTitle"
            />
        </label>
        <button class="btn btn-danger" @click="handleDelete(todoObj.id)">删除</button>
        <button v-show="!todoObj.isEdit" class="btn btn-edit" @click="handleEdit(todoObj)">编辑</button>
    </li>
</template>

<script>
    export default {
        name: 'MyItem',
        props: ['todoObj'],
        methods: {
            // 勾选or取消勾选
            handleCheck() {
                this.$bus.$emit('checkTodo', this.todoObj.id)
                // this.checkTodo(this.todoObj.id)
            },
            // 删除
            handleDelete(id) {
                if(confirm('确定删除该项目？'))
                this.$bus.$emit('deleteTodo', id)
                // this.deleteTodo(id)
            },
            // 编辑
            handleEdit(todoObj) {
                if(todoObj.hasOwnProperty('idEdit')) {
                    todoObj.isEdit = true
                } else {
                    this.$set(todoObj, 'isEdit', true)
                }
                this.$nextTick(function() {
                    this.$refs.inputTitle.focus()
                })
            },
            // 失去焦点回调（真正执行修改逻辑）
            handleBlur(todo, e) {
                this.todoObj.isEdit = false
                if(!e.target.value.trim()) return alert('输入不能为空！！！')
                this.$bus.$emit('updataTodo', todo.id, e.target.value)
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