<script>
    import {createEventDispatcher} from "svelte";

  const numbers = Array.from({ length: 10 }, (v, i) => i + 1);

  let selected = 10;

  const dispatcher = createEventDispatcher();

  const onChange = (e) => {
    selected = e.target.value;
    dispatcher("rating-select", selected);
  };
</script>

<ul class="rating">
  {#each numbers as n (n)}
    <li>
      <input
        type="radio"
        id={"num" + n}
        name="rating"
        value={n}
        on:change={onChange}
        checked={selected === n}
      />
      <label for={"num" + n}>{n}</label>
    </li>
  {/each}
</ul>

<style>
  .rating {
    display: flex;
    align-items: center;
    justify-content: space-around;
    margin: 30px 0px;
  }
  .rating li {
    position: relative;
    background: #f4f4f4;
    width: 50px;
    height: 50px;
    padding: 10px;
    border-radius: 50%;
    text-align: center;
    font-size: 19px;
    border: 1px #eee solid;
    transition: 0.3s;
  }

  .rating li label {
    position: absolute;
    top: 50%;
    left: 50%;
    width: 50px;
    height: 50px;
    padding: 10px;
    border-radius: 50%;
    transform: translate(-50%, -50%);
    cursor: pointer;
  }

  .rating li:hover {
    background: #ff6a95;
    color: #fff;
  }

  [type="radio"] {
    opacity: 0;
  }

  [type="radio"]:checked ~ label {
    background: #ff6a95;
    color: #fff;
  }
</style>
