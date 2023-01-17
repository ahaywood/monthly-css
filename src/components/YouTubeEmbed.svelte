<script lang="ts">
  import { onMount } from "svelte";

  interface VideoDimensions {
    height: number;
    width: number;
  }

  export let videoId: string = "";
  let videoDimensions: VideoDimensions = {} as VideoDimensions;
  let aspectRatio: string = "";

  const getWidthHeight = () => {
    const url = `https://noembed.com/embed?url=https://www.youtube.com/watch?v=${videoId}`;
    fetch(url)
      .then((response) => response.json())
      .then((data) => {
        const { width, height } = data;
        videoDimensions = { height, width };
      })
      .then(
        () =>
          (aspectRatio = `${
            (videoDimensions.height / videoDimensions.width) * 100
          }%`)
      );
  };

  onMount(() => {
    getWidthHeight();
  });
</script>

<div style={`padding-top: ${aspectRatio}`} class="w-full h-0 relative mb-5">
  <iframe
    class="absolute top-0 left-0 right-0 bottom-0 w-full h-full"
    src={`https://www.youtube.com/embed/${videoId}`}
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
    title="youtube"
    allowfullscreen={true}
  />
</div>
