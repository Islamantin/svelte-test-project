<script>
  import { v4 as uuidv4 } from "uuid";
  import { FeedbackStore } from "../stores";
  import Card from "./view/Card.svelte";
  import Button from "./view/Button.svelte";
  import RatingSelect from "./RatingSelect.svelte";

  let text = "";
  let rating = 10;

  let btnDisabled = true;
  let min = 10;
  let message = null;

  const handleInput = (e) => {
    text = e.target.value;
    const length = text.trim().length;
    if (length <= 0) {
      message = null;
      btnDisabled = true;
    } else if (length < min) {
      message = `Text must be at least ${min} characters`;
      btnDisabled = true;
    } else {
      message = null;
      btnDisabled = false;
    }
  };

  const handleSelect = (e) => (rating = e.detail);

  const handleSubmit = (e) => {
    const length = text.trim().length;
    if (length >= min) {
      const newFeedback = {
        id: uuidv4(),
        text,
        rating: +rating,
      };
      FeedbackStore.update((current) => [newFeedback, ...current]);
      text = "";
      handleInput({ target: { value: "" } });
    }
  };
</script>

<Card>
  <header>
    <h2>How would you rate our service?</h2>
  </header>
  <form on:submit|preventDefault={handleSubmit}>
    <RatingSelect on:rating-select={handleSelect} />
    <div class="input-group">
      <input
        type="text"
        on:input={handleInput}
        placeholder="Tell us something"
        value={text}
      />
      <Button disabled={btnDisabled} type="submit">Send</Button>
    </div>
    {#if message}
      <div class="message">{message}</div>
    {/if}
  </form>
</Card>

<style>
  header {
    max-width: 400px;
    margin: auto;
  }
  header h2 {
    font-size: 22px;
    font-weight: 600;
    text-align: center;
  }

  .input-group {
    display: flex;
    flex-direction: row;
    border: 1px #ccc solid;
    padding: 8px 10px;
    border-radius: 8px;
    margin-top: 15px;
  }

  input {
    flex-grow: 2;
    border: none;
    font-size: 16px;
  }

  input:focus {
    outline: none;
  }

  .message {
    padding-top: 10px;
    text-align: center;
    color: rebeccapurple;
  }
</style>
