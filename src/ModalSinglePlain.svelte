{#if show}
  <div class=modal-overlay tabindex=-1 data-close on:click={overlay_click} transition:fade={{duration: 180}} use:focus_trap use:append_to={document.body}>
    <slot/>
  </div>
{/if}

<svelte:window on:popstate={popstate} on:keydown={e => { if (e.key === 'Escape') show = false }}/>

<script>
import {fade} from 'svelte/transition'
import {focus_trap, append_to} from 'components/src/util.js'

export let show

$: set_body_scroll(show)
$: if (show)
    history.pushState(null, null, '')

function popstate() {
    if (show)
        show = false
}
function overlay_click(e) {
    if ('close' in e.target.dataset)
        show = false
}
function set_body_scroll(shown) {
    document.documentElement.classList.toggle('open-modal', shown)
}
</script>

<style>
/* Non-iOS */
:global(html.open-modal body) {
  touch-action: none;
  overflow: hidden;
  height: 100vh;
}
/* iOS */
:global(html.open-modal) .modal-overlay {
  touch-action: none;
}
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  z-index: 10;
  overflow-y: auto;
  -webkit-overflow-scrolling: touch;
  display: flex;
  flex-direction: column;
  justify-content: center;
}
</style>
