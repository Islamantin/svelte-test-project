<script>
  import { FeedbackStore } from "../stores";
  import { scale } from "svelte/transition";
  import FeedbackItem from "./FeedbackItem.svelte";
</script>

{#each $FeedbackStore as fb (fb.id)}
  <div in:scale out:scale={{ duration: 300 }}>
    <FeedbackItem item={fb} />
  </div>
{/each}
