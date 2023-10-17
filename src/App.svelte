<script lang="ts">
  import { Canvas } from "@threlte/core";
  import Scene from "./lib/scene.svelte";
  import { World } from "@threlte/rapier";
</script>

<Canvas>
  <World gravity={[0, 0, 0]}>
    <Scene />
  </World>
</Canvas>
