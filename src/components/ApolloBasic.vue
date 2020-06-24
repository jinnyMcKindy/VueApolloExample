<template>
    <div>
        <div>
            Limit <input v-model.number="options.paginate.limit"/>
        </div>
        <div>
            <p>Title: <input v-model="input.title" /></p>
            <p>Body: <input v-model="input.body" /></p>
        </div>
        <ApolloMutation
            :mutation="require('../graphql/createPost.gql')" 
            :variables="{ input }"
             @done="onDone"
        >
            <template v-slot="{ mutate, loading, error }">
                <button :disabled="loading" @click="mutate()">Click me</button>
                <p v-if="error">An error occurred: {{ error }}</p>
            </template>
        </ApolloMutation>
       <!--<ApolloQuery 
            :query="require('../graphql/getPost.gql')" 
            :variables="{ postID }">
            <template v-slot="{ result: { loading, error, data } }">
                <div v-if="error" class="error apollo">{{error}}</div>
                <div v-else-if="data" class="result apollo">
                    <p>{{data.post.title}}</p>
                    <p>{{data.post.body}}</p>
                </div>
            </template>
        </ApolloQuery>-->
        <ApolloQuery 
            :query="require('../graphql/allPosts.gql')" 
            :variables="{ options }">
            <template v-slot="{ result: { loading, error, data } }">
            <div v-if="loading" class="loading apollo">Loading...</div>
            <div v-else-if="error" class="error apollo">An error occurred</div>
            <div v-else-if="data" class="result apollo">
                <div v-for="post in data.posts.data" :key="post.id">
                    <h4>{{post.title}}</h4>
                    <p>{{post.body}}</p>
                </div>
            </div>
            <div v-else class="no-result apollo">No result :(</div>
            </template>
        </ApolloQuery>
        <div>
            <span 
                v-for="page in 11" 
                :key="page"
                @click="changePage(page)">
                {{page}}
            </span>
        </div>
    </div>
</template>
<script>

export default {
    name: "ApolloBasic",
    data () {
        return {
            postID: 1,
            input: {
                title: "A Very Captivating Post Title",
                body: "Some interesting content."
            },
            options: {
                paginate: {
                    page: 1,
                    limit: 10
                }
            }
        }
    },
    methods: {
        changePage(page) {
            const opt = {...this.options}
            opt.paginate.page = page;
            this.options = opt;
            this.$apollo.queries.posts.refetch()
        },
        onDone() {
            alert('saved')
            this.input.title = '';
            this.input.body = '';
            this.$apollo.queries.posts.refetch()
        }
    }
}
</script>