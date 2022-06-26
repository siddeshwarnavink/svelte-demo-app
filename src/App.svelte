<script>
  import Layout from "./components/Layout/Layout.svelte";
  import PostList from "./components/Post/PostList.svelte";
  import PostItem from "./components/Post/PostItem.svelte";

  let postList = [
    { id: 1, title: "Post title", content: "Post content" },
    { id: 2, title: "Post title 2", content: "Post content 2" },
  ];

  function editPostHandler(event) {
    postList = postList.map(post => {
      if(post.id !== event.detail.id) return post;

      return {
        ...post,
        title: event.detail.title,
        content: event.detail.content
      }
    })
  }

  function deletePostHandler(event) {
    postList = postList.filter(post => post.id !== event.detail)
  }
</script>

<Layout>
  <h1>Hello!</h1>

  <PostList>
    {#each postList as postItem}
      <PostItem
        id={postItem.id}
        title={postItem.title}
        content={postItem.content}
        on:editPost={editPostHandler}
        on:deletePost={deletePostHandler}
      />
    {/each}
  </PostList>
</Layout>
