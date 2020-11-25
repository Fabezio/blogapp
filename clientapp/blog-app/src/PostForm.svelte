<script>
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();
  let data = {
    title: "",
    category: "",
    author: "",
    content: "",
  };
  
  const baseUrl = "http://localhost:4000/blog";
  let addNewPost = async () => {
    if (
      data.title.trim() === "" ||
      data.category.trim() === "" ||
      data.author.trim() === "" ||
      data.content.trim() === ""
    ) {
      return;
    }
    const res = await fetch(`${baseUrl}`, {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(data),
    });
    const post = res.json();
    dispatch("postCreated", post);
  };
  let deletePost = async (id) => {
    if (confirm("Are You Sure?")) {
      let res = await fetch(`${baseUrl}/${id}`, {
        method: "DELETE",
      });
      posts = posts.filter((p) => p._id !== id);
    } else {
      alert("Your Post is safe!!");
    }
  };
</script>

<section class="mt-4">
  <div class="container">
    <div class="row">
      <div class="col-md-5">
        <div class="card p-3">
          <form on:submit|preventDefault={addNewPost}>
            <div class="form-group">
              <label for="title">Title</label>
              <input
                bind:value={data.title}
                type="text"
                class="form-control"
                id="text"
                placeholder="Title" />
            </div>
            <div class="form-group">
              <label for="title">Category</label>
              <input
                bind:value={data.category}
                type="text"
                class="form-control"
                id="text"
                placeholder="Category" />
            </div>
            <div class="form-group">
              <label for="title">Author</label>
              <input
                bind:value={data.author}
                type="text"
                class="form-control"
                id="text"
                placeholder="Author" />
            </div>
            <div class="form-group">
              <label for="content">Content</label>
              <textarea
                bind:value={data.content}
                class="form-control"
                id="content"
                rows="3"
                placeholder="Content" />
            </div>
            <button type="submit" class="btn btn-primary">Add Note</button>
            
          </form>
        </div>
      </div>
    </div>
  </div>
</section>
