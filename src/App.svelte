<script>
  import { onMount } from "svelte";

  import Layout from "./components/Layout/Layout.svelte";
  import PostList from "./components/Post/PostList.svelte";
  import PostItem from "./components/Post/PostItem.svelte";
  import Modal from "./components/UI/Modal.svelte";
  import Button from "./components/UI/Button.svelte";
  import PostEditor from "./components/Post/PostEditor.svelte";
  import updateArray from "./util/updateArray";
import Spinner from "./components/UI/Spinner.svelte";

  let loading = false;
  let postList = [];
  let isEditModalOpen = false;

  onMount(async () => {
    loading = true;
    const response = await fetch("https://jsonplaceholder.typicode.com/posts");
    postList = await response.json().then((postList) =>
      postList.map(({body, ...otherPostdata}) => ({
        ...otherPostdata,
        content: body,
      }))
    );
    loading = false;
  });

  function toggleEditModalHandler() {
    isEditModalOpen = !isEditModalOpen;
  }

  function editPostHandler(event) {
    postList = postList.map((post) => {
      if (post.id !== event.detail.id) return post;

      return {
        ...post,
        title: event.detail.title,
        content: event.detail.content,
      };
    });
  }

  function deletePostHandler(event) {
    postList = postList.filter((post) => post.id !== event.detail);
  }

  function createPostHandler(event) {
    toggleEditModalHandler();

    postList = updateArray(
      [
        {
          id: postList[postList.length - 1].id + 1,
          title: event.detail.title,
          content: event.detail.content,
        },
      ],
      postList
    );
  }
</script>

<Layout>
  <Modal isOpen={isEditModalOpen} on:close={toggleEditModalHandler}>
    <h3>Create post</h3>

    <PostEditor
      on:submit={createPostHandler}
      on:cancel={toggleEditModalHandler}
    />
  </Modal>

  {#if loading}
    <Spinner />
  {:else}
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
  {/if}

  <Button fab on:click={toggleEditModalHandler}>+</Button>
</Layout>
