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
                style="display: none"
            >Create post
            </my-button>
            <my-select
                v-model="selectedSort"
                :options="sortOptions"
            />
        </div>
        <my-dialog v-model:show="dialogVisble">
            <post-form
            />
        </my-dialog>

        <post-list
            :posts="sortedAndSearchedPosts"
            v-if="!isPostsLoading"
        />
        <div v-else>Loading in progress...</div>
    </div>
</template>

<script>
import PostList from '@/components/PostList.vue';
import PostForm from '@/components/PostForm.vue';
import axios from 'axios';
import {ref} from 'vue';
import MyButton from "@/components/UI/MyButton";
import useSortedPosts from "@/hooks/useSortedPosts";
import useSortedAndSearchedPosts from "@/hooks/useSortedAndSearchedPosts"
import {usePosts} from "@/hooks/usePosts";

export default {
    components: {
        MyButton,
        PostList, PostForm
    },

    data() {
        return {
            dialogVisble: false,
            sortOptions: [
                {value: 'title', name: 'By name'},
                {value: 'body', name: 'By descriprion'},
            ]
        }
    },
    setup(props) {
        const {posts, totalPages, isPostsLoading} = usePosts(10);
        const {sortedPosts, selectedSort} = useSortedPosts(posts);
        const {searchQuery, sortedAndSearchedPosts} = useSortedAndSearchedPosts(sortedPosts);

        return {
            posts,
            totalPages,
            isPostsLoading,
            sortedPosts,
            selectedSort,
            searchQuery,
            sortedAndSearchedPosts
        }
    },

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