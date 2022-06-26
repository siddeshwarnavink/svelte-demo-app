<script>
  import { onMount } from "svelte";
  import { fade, fly } from "svelte/transition";
  import { flip } from "svelte/animate";

  import Layout from "./components/Layout/Layout.svelte";
  import PostList from "./components/Post/PostList.svelte";
  import PostItem from "./components/Post/PostItem.svelte";
  import Modal from "./components/UI/Modal.svelte";
  import Button from "./components/UI/Button.svelte";
  import PostEditor from "./components/Post/PostEditor.svelte";
  import updateArray from "./util/updateArray";
  import Spinner from "./components/UI/Spinner.svelte";

  let postListLoading = false;
  let postList = [];
  let isEditModalOpen = false;

  onMount(async () => {
    postListLoading = true;
    const response = await fetch("https://jsonplaceholder.typicode.com/posts");
    postList = await response.json().then((postList) =>
      postList.slice(0, 10).map(({ body, ...otherPostdata }) => ({
        ...otherPostdata,
        content: body,
      }))
    );
    postListLoading = false;
  });

  function toggleEditModalHandler() {
    isEditModalOpen = !isEditModalOpen;
  }

  const editPostHandler = async (event) => {
    postList = postList.map((post) => {
      if (post.id !== event.detail.id) return post;

      return {
        ...post,
        title: event.detail.title,
        content: event.detail.content,
      };
    });

    await fetch(
      `https://jsonplaceholder.typicode.com/posts/${event.detail.id}`,
      {
        method: "UPDATE",
        body: JSON.stringify({
          title: event.detail.title,
          body: event.detail.content,
        }),
        headers: {
          "Content-type": "application/json",
          "Access-Control-Allow-Methods": "UPDATE",
        },
      }
    );
  };

  const deletePostHandler = async (event) => {
    postList = postList.filter((post) => post.id !== event.detail);

    await fetch(`https://jsonplaceholder.typicode.com/posts/${event.detail}`, {
      method: "DELETE",
      headers: {
        "Content-type": "application/json",
        "Access-Control-Allow-Methods": "DELETE",
      },
    });
  };

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

  {#if postListLoading}
    <Spinner />
  {:else}
    <PostList>
      {#each postList as postItem (postItem.id)}
        <div animate:flip in:fly={{ y: 200, duration: 300 }} out:fade>
          <PostItem
            id={postItem.id}
            title={postItem.title}
            content={postItem.content}
            on:editPost={editPostHandler}
            on:deletePost={deletePostHandler}
          />
        </div>
      {/each}
    </PostList>
  {/if}

  <Button fab on:click={toggleEditModalHandler}>+</Button>
</Layout>
