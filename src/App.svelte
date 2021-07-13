<script>
    import { onMount } from 'svelte';
    import mitt from 'mitt';

    const getInitialState = () => {
        try {
            const savedState = sessionStorage.getItem("PolyglotSPAState");
            if (!savedState) {
                return undefined;
            }
            return JSON.parse(savedState);
        }  catch (e) {
           return undefined;
        }
    }

    const saveState = (state) => {
        try {
            const stringifyState = JSON.stringify(state);
            sessionStorage.setItem("PolyglotSPAState", stringifyState);
        } catch (e) {}
    }

    const newState = {
        helloArray: []
    }

  onMount(() => {
      const emitter = mitt();
      window.emitter = emitter;
      const state = getInitialState() || newState;
      console.log(state);
      emitter.on('hello', e => {
          state.helloArray.push('Hello from ' + e);
          saveState(state);
      });
  });
</script>

