<script>
  import { todos, autoincrement } from '$lib/stores'
  import { onMount } from 'svelte'

  let new_todo

  let placeholders = [
    "Salvar al mundo 🦸‍♂️",
    "Apagar los frejoles ❗",
    "Lanzarse un bailesote 🕺",
    "Rescatar gatitos 😿",
    "Comprar milanesas 🛒",
    "Pagar la luz 💡",
    "Rascarse el ombligo 😋",
    "Demostrarle a Alan García lo que pidió 😈"
  ]

  function pickRand() {
    let random = Math.floor(Math.random() * placeholders.length)
    let placeHold = placeholders[random]
    return placeHold
  }
  
  let ph
  onMount(() => {
    ph = pickRand()
  })
  const crearTodo = (e) => {
    ph = pickRand()
    $autoincrement = $autoincrement + 1
    $todos = [...$todos,{
      id: $autoincrement,
      text: new_todo,
      done: false
    }]
    e.target.reset()
    console.log($todos, $autoincrement)
  }
</script>

<article>
  <form on:submit|preventDefault={crearTodo} action="/" class="formadd flex">
    <label for="todo">
      ✔️ Añade una tarea:
    </label>
    <div class="active flex">  
      <input type="text" id="todo" name="todo" placeholder={ph} bind:value={new_todo} autocomplete="off" required>
      <button type="submit" class="crear">CREAR</button>
    </div>
  </form>

  <ul>
    {#each $todos as todo}
      <li>
        <form on:submit|preventDefault={() => {
          $todos = $todos.filter(t => t.id !== todo.id)
        }}
        class="tarea">
          <div class="izq">
            <input type="checkbox" name="done" id={todo.id}>
            <label for={todo.id} done={todo.done}>{todo.text}</label>
            <span></span>
          </div>
          <button type="submit">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 30 30" width="30px" height="30px"><path d="M 7 4 C 6.744125 4 6.4879687 4.0974687 6.2929688 4.2929688 L 4.2929688 6.2929688 C 3.9019687 6.6839688 3.9019687 7.3170313 4.2929688 7.7070312 L 11.585938 15 L 4.2929688 22.292969 C 3.9019687 22.683969 3.9019687 23.317031 4.2929688 23.707031 L 6.2929688 25.707031 C 6.6839688 26.098031 7.3170313 26.098031 7.7070312 25.707031 L 15 18.414062 L 22.292969 25.707031 C 22.682969 26.098031 23.317031 26.098031 23.707031 25.707031 L 25.707031 23.707031 C 26.098031 23.316031 26.098031 22.682969 25.707031 22.292969 L 18.414062 15 L 25.707031 7.7070312 C 26.098031 7.3170312 26.098031 6.6829688 25.707031 6.2929688 L 23.707031 4.2929688 C 23.316031 3.9019687 22.682969 3.9019687 22.292969 4.2929688 L 15 11.585938 L 7.7070312 4.2929688 C 7.5115312 4.0974687 7.255875 4 7 4 z"/></svg>
          </button>
        </form>
      </li>
    {/each}
  </ul>
</article>