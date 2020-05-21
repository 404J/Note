---
title: 'Vue: demo'
date: 2020-05-06 19:51:54
tags: [Vue]
published: true
hideInList: false
feature: http://404j-images.test.upcdn.net/coverImage/vue-demo.png
isTop: false
---
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>

<body>
    <div id="app">
        <p>message: {{ message }}</p>
        <p>reversedMessageComputed: {{ reversedMessageComputed }}</p>
        <p>reversedMessageMethod: {{ reversedMessageMethod() }}</p>
        <p>reversedMessageWatch: {{ reversedMessageWatch }}</p>
        <input v-model="message" />
        <hr>
        <template v-if="message === 'username'">
            <label>Username</label>
            <input placeholder="Enter your username" key="username-input">
        </template>
        <template v-else>
            <label>Email</label>
            <input placeholder="Enter your email address" key="email-input">
        </template>

        <ul>
            <li v-for="(value, index) in persons">
                {{ index }}: {{ value }}
            </li>
        </ul>

        <ul>
            <li v-for="value of persons">
                {{ value }}
            </li>
        </ul>

        <ul>
            <li v-for="(value, name, index) of personMars">
                {{ index }}. {{ name }}: {{ value }}
            </li>
        </ul>

        <button @click="clickBtn({message: 'message', event: $event})">{{ message }}</button>

        <form @submit.prevent="onSubmit">
            <button>submit</button>
        </form>

        <span style="color: red" @click="clickSpanOne">
            span1
            <span style="color: black" @click.stop="clickSpanTwo">span2</span>
        </span>
        <hr>
        <input @keyup.enter="clickEnter" />
        <hr>
        <div :style="{ fontSize: postFontSize + 'em' }">
            <blog v-for="post of posts" :key="post.id" :post="post" @enlarge-text="enlargeText" />
        </div>
        <hr>
        <input v-model="searchText" />
        <hr>
        <input :value="searchTextSelf" @input="searchTextSelf=$event.target.value" />
        <hr>
        <custom-input :value="searchTextCustom" @input="searchTextCustom=$event"></custom-input>
        <custom-input v-model="searchTextCustom"></custom-input>
        <hr>
        <my-component :init-count="initCount" style="color: red"></my-component>
        <hr>
        <base-input v-model="baseInput"></base-input>
    </div>
</body>
<script>
    Vue.component('blog', {
        data: function () {
            return {
                changedPost: this.post
            }
        },
        props: {
            post: Object
        },
        methods: {
            changePost() {
                this.changedPost = {
                    title: 'changed title',
                    content: 'changed content'
                }
            }
        },
        template: `
            <div>
                <div>{{ changedPost.title }}</div>
                <div>{{ changedPost.content }}</div>
                <button v-on:click="$emit('enlarge-text', 1, 'test')">
                    Enlarge text
                </button>
                <button v-on:click="changePost">
                    changePost
                </button>
                <hr>
                -----------------------------
            </div>
        `
    })

    Vue.component('custom-input', {
        data: function () {
            return {
            }
        },
        props: [
            'value'
        ],
        template: `
            <input
                v-bind:value="value"
                v-on:input="$emit('input', $event.target.value)"
            />
        `
    })

    let myComponent = {
        data() {
            return {
                countAdd: this.initCount + 1
            }
        },
        props: {
            initCount: Number
        },
        template: `
            <div class="my-component">
                my test component
                <br>
                {{ countAdd }}
            </div>
        `
    }

    let baseInput = {
        inheritAttrs: false,
        props: ['label', 'value'],
        computed: {
            inputListeners: function () {
                var vm = this
                return Object.assign({},
                    this.$listeners,
                    {
                        input: function (event) {
                            vm.$emit('input', event.target.value)
                        },
                        focus: function () {
                            console.log('Focus!')
                        }
                    }
                )
            }
        },
        template: `
            <label>
            {{ label }}
            <input
                v-bind="$attrs"
                v-bind:value="value"
                v-on="inputListeners"
            >
            </label>
        `
    }

    const app = new Vue({
        el: '#app',
        components: {
            myComponent,
            baseInput
        },
        data: {
            message: 'Hello Vue!',
            reversedMessageWatch: '',
            loginType: 'username',
            persons: [
                'mars',
                'leo',
                'dale'
            ],
            personMars: {
                name: 'mars',
                age: 24,
                country: 'china'
            },
            posts: [
                { id: 1, title: 'Vue 组件', content: '这是一个内容' },
                { id: 2, title: 'Vue 组件', content: '这是一个内容' }
            ],
            postFontSize: 1,
            searchText: 'hello mars',
            searchTextSelf: 'hello mars',
            searchTextCustom: 'hello mars',
            initCount: 1,
            baseInput: 'This is base input'
        },
        computed: {
            reversedMessageComputed() {
                return this.message.split('').reverse().join('')
            }
        },
        methods: {
            reversedMessageMethod() {
                return this.message.split('').reverse().join('')
            },
            clickBtn({ message, event }) {
                console.log({ message, event })
            },
            onSubmit() {
                console.log('Submit')
            },
            clickSpanOne() {
                console.log('clickSpanOne')
            },
            clickSpanTwo() {
                console.log('clickSpanTwo')
            },
            clickEnter() {
                console.log('click enter')
            },
            enlargeText(enlargeAmount, test) {
                console.log(test)
                this.postFontSize += enlargeAmount
            }
        },
        watch: {
            message() {
                this.reversedMessageWatch = this.message.split('').reverse().join('')
            }
        }
    })
</script>

</html>
```