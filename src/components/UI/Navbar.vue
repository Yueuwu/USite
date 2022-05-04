<template>
    <div class="navbar">
        <div @click="$router.push('/')" class="mark">Vue 3</div>
        <div class="navbar__btns">
            <my-button @click="$router.push('/posts')" style="background-color: #fff;">Posts</my-button>
            <my-button @click="$router.push('/about')" style="margin-left: 20px; background-color: #fff;">About</my-button>
            <my-button @click="$router.push('/store')" style="margin-left: 20px; background-color: #fff;">Store</my-button>
            <my-button @click="$router.push('/composition')" style="margin-left: 20px; background-color: #fff;">CompositionAPI</my-button>
        </div>
    </div>
</template>

<script>
import MyButton from "@/components/UI/MyButton";

export default {
    components: {MyButton}


}
</script>

<style scoped>
    .navbar {
        height: 50px;
        background-color: gray;
        box-shadow: 2px 2px 4px gray;
        display: flex;
        align-items: center;
        padding: 0 15px;
    }
    .navbar__btns{
        margin-left: auto;

    }
    .mark {
        font-size: 17px;
        font-weight: bold;
        border: 1px solid teal;
        padding: 10px 15px;
        background-color: #fff;
    }
</style>