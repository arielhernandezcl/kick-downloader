<script>
  let videoUrl = '';
  let result = null;
  let title = '';
  let img = '';
  let channel = '';
  let followers = '';
  let categoryName = '';

  async function handleSubmit(event) {
    event.preventDefault();

    const apiUrl = convertToApiUrl(videoUrl);

    try {
      const response = await fetch(apiUrl);
      const data = await response.json();
      const source = data.source;
      result = `${source}`;
      title = data.livestream.session_title;
      img = data.livestream.thumbnail;
      channel = data.livestream.channel.user.username;
      followers = data.livestream.channel.followersCount;
      categoryName = data.livestream.categories[0].name;
      console.log(data);
    } catch (error) {
      console.error(error);
      result = 'Error al obtener la fuente del video.';
    }
  }

  function convertToApiUrl(videoUrl) {
    const videoId = videoUrl.split('/').pop();
    return `https://kick.com/api/v1/video/${videoId}`;
  }
</script>

<main>
  <a href="/"><img src="https://uc25ea79b8c7e7022dafc49044ee.previews.dropboxusercontent.com/p/thumb/AB-4Dsek6B67aqKV8Uioju5BUrr_CtzbCFKYv9q6IIosyrH9mqBWL1tpO3bzCEw9jUNcKiVX8AoKGqb_3FkzsmA58QBrANoWOQCwg5K-apObH82vZr3lFXoOd48TjbpUylVnWyeKMoX6iPHd6WZJtAurzKjjYa60ZSdiAgid9U3utbTy0N-JMh_-z4pkvEKEU2XJWYkPD43rPkRFulHRL7KQj4zdnNv6cX-xXym37PBnvr8jToqrcGMitK9N-TxBhjZI0epmJR77fRqKrpclTKPxhDTWUKzTfWeSpsQ-kvpYq8k26hNw4sqy-NTrdO9EFKyJaKd5Elvv8NVUkM_B5iJw/p.png" width="100" alt=""></a>
  <h3>Kick VOD M3U8</h3>
  <form on:submit={handleSubmit}> 
    <div class="input-group input-group-lg">
      <span class="input-group-text" id="inputGroup-sizing-lg">Url VOD</span>
      <input type="url" class="form-control" aria-label="Sizing example input" aria-describedby="inputGroup-sizing-lg" id="videoUrl" bind:value={videoUrl} required>
      <button class="btn btn-light" type="submit">Obtener M3U8</button>
    </div>
  </form>
  
  {#if result !== null}
    <div class="card">
      <img src="{img}" class="card-img-top" alt="{title}" loading="lazy">
      <div class="card-body">
        <h4 class="card-title">{title}</h4>
      </div>
      <ul class="list-group list-group-flush">
        <li class="list-group-item"><a href={`https://kick.com/${channel}`} class="card-link">{channel}</a></li>
        <li class="list-group-item">Followers: {followers}</li>
        <li class="list-group-item">Category: {categoryName}</li>
      </ul>
      <div class="card-body">
        <span class="input-group-text" id="inputGroup-sizing-lg">M3u8 Url</span>
        <input id="result" type="text" class="form-control" aria-label="Sizing example input" aria-describedby="inputGroup-sizing-lg" value={result} readonly>
      </div>
    </div>
  {/if}
</main>

<style>
  .input-group-text {
  margin-bottom: 0.5rem;
  max-width: 110px;
  font-size: 1.3rem;
  line-height: 1.5;
  background-color: rgb(75, 73, 73);
  color: azure;
  
}

.form-control {
  width: fit-content;
  font-size: 1.3rem;
  line-height: 1.5;
  border: 1px solid #ced4da;
  border-radius: 0.25rem;
  margin-bottom: 0.5rem;
  background-color: rgb(75, 73, 73);
  color:  aliceblue;
}

div {
  display: flex;
  align-items: center;
  justify-content: center;
}

button {
  
  margin-bottom: auto;
  border-radius: 0.25rem;
  background-color: black;
  color: azure;
}

.card {
  display: flex;
  margin-top: 2rem;
  margin-bottom: 2rem;
  background-color: rgb(75, 73, 73);
  color: azure;
  font-size: 1.3rem;
  
}

.card-body {
  margin-bottom: 1rem;
  background-color: rgb(75, 73, 73);
  color: azure;
  
  }

.list-group-item {
  background-color: rgb(75, 73, 73);
  color: azure;
}

.list-group {
  background-color: rgb(75, 73, 73);
  color: azure;
}

.card-img-top {
  width: 50rem;
}
</style>
