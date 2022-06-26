<script>
  import Layout from "./components/Layout/Layout.svelte";
  import PostList from "./components/Post/PostList.svelte";
  import PostItem from "./components/Post/PostItem.svelte";

  let postList = [
    { id: 1, title: "Post title", content: "Post content" },
    { id: 2, title: "Post title 2", content: "Post content 2" },
  ];
</script>

<Layout>
  <h1>Hello!</h1>

  <PostList>
    {#each postList as postItem}
      <PostItem title={postItem.title} content={postItem.content} />
    {/each}
  </PostList>
</Layout>
