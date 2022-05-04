<template>
    <div >
        <h1>Page with posts</h1>
        <my-input
            v-focus
            v-model="searchQuery"
            placeholder="Search by name"
        />
        <div class="app__btns">
            <my-button
                style=""
                @click="showDialog"
            >Create post
            </my-button>
            <my-select
                v-model="selectedSort"
                :options="sortOptions"
            />
        </div>
        <my-dialog v-model:show="dialogVisble">
            <post-form
                @create="createPost"
            />
        </my-dialog>

        <post-list
            :posts="sortedAndSearchPosts"
            @remove="removePost"
            v-if="!isPostsLoading"
        />
        <div v-else>Loading in progress...</div>
        <div v-intersection="loadMorePosts" class="observer"></div>
        <!--        <div class="page__wrapper">-->
        <!--            <div-->
        <!--            v-for="pageNumber in totalPages"-->
        <!--            :key="pageNumber"-->
        <!--            class="page"-->
        <!--            :class="{-->
        <!--                'current-page': page === pageNumber-->
        <!--            }"-->
        <!--            @click="changePage(pageNumber)"-->
        <!--            >-->
        <!--                {{ pageNumber }}-->
        <!--            </div>-->
        <!--        </div>  -->
    </div>
</template>

<script>
import PostList from '@/components/PostList.vue';
import PostForm from '@/components/PostForm.vue';
import axios from 'axios';

export default {
    components: {
        PostList, PostForm
    },

    data() {
        return {
            posts: [],
            dialogVisble: false,
            isPostsLoading: false,
            selectedSort: '',
            searchQuery: '',
            page: 1,
            limit: 10,
            totalPages: 0,
            sortOptions: [
                {value: 'title', name: 'By name'},
                {value: 'body', name: 'By descriprion'},
            ]
        }
    },
    methods: {
        createPost(post) {
            this.posts.push(post);
            this.dialogVisble = false;
        },
        removePost(post) {
            this.posts = this.posts.filter(p => p.id !== post.id)
        },
        showDialog() {
            this.dialogVisble = true;
        },
        // changePage(pageNumber){
        //     this.page = pageNumber;
        //     //this.fetchPosts();
        // },
        async fetchPosts() {
            try {
                this.isPostsLoading = true;
                const response = await axios.get( 'https://jsonplaceholder.typicode.com/posts', {
                    params: {
                        _page: this.page,
                        _limit: this.limit,
                    }
                });
                this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit);
                this.posts = response.data;
            } catch (e) {
                alert('Error')
            } finally {
                this.isPostsLoading = false;
            }
        },
        async loadMorePosts() {
            try {
                this.page++

                const response = await axios.get( 'https://jsonplaceholder.typicode.com/posts', {
                    params: {
                        _page: this.page,
                        _limit: this.limit,
                    }
                });
                this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit);
                this.posts = [...this.posts, ...response.data];
            } catch (e) {
                alert('Error')
            }
        }
    },
    mounted() {
        this.fetchPosts();

    },
    computed: {
        sortedPosts() {
            return [...this.posts].sort((post1, post2) => {
                return post1[this.selectedSort]?.localeCompare(post2[this.selectedSort])
            })
        },
        sortedAndSearchPosts() {
            return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))

        }

    },
    watch: {
        // page() {
        //     this.fetchPosts();
        // }
    }
}
</script>

<style>

.app__btns {
    display: flex;
    justify-content: space-between;
    margin: 15px 0 15px 0;
}
.page__wrapper {
    display: flex;
    margin-top: 15px;
}
.page {
    border: 1px solid #000;
    padding: 10px;
}
.current-page {
    border: 2px solid teal;
}
.observer{
    /*   */
}

</style>