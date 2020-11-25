<script>
  import { onMount } from "svelte";
  import PostForm from "./PostForm.svelte";
  const baseUrl = "http://localhost:8080/blog";
  let posts = [];
  onMount(async () => {
    let res = await fetch(baseUrl);
    posts = await res.json();
    console.log(posts);
  });
  let addpost = ({ detail: post }) => {
    posts = [post, ...posts];
  };
  let editPost = {
    title: "",
    category: "",
    author: "",
    content: "",
    id: null,
  };
  let editPostDetails = async (post) => {
    editPost = post;
    console.log(post);
  };
</script>
<section class="mt-5">
  <div class="container">
    <div class="row">
      {#if posts.length === 0}
      <p>Loading</p>
      {:else}
      {#each posts as post}
      <div class="col-md-4">
        <div class="card">
          <div class="card-header">{post.category}</div>
          <div class="card-body">
            <h5 class="card-title">{post.title}</h5>
            <p class="card-text">{post.content}</p>
            <button class="btn btn-info">Edit</button>
            <button class="btn btn-danger">Delete</button>
          </div>
        </div>
      </div>
      {/each}
      {/if}
    </div>
  </div>
</section>
<PostForm on:postCreated={addpost} >
<button
              class="btn btn-danger"
              on:click={deletePost(post._id)}>Delete</button>
            <button
              class="btn btn-info"
              on:click={editPostDetails(post)}>Edit</button>
</PostForm>