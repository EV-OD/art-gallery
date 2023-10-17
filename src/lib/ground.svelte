<script lang="ts">
  import { T } from "@threlte/core";
  import { AutoColliders, RigidBody } from "@threlte/rapier";
  import GroundModel from "./3d-models/groundModel.svelte";
  import Model from "./3d-models/model.svelte";
  import { afterUpdate } from "svelte";
  import Art from "./3d-models/art.svelte";
  export let loaded: boolean;
</script>

<Art bind:loaded />

<!-- <T.Group position={[0, -0.5, 0]}>
  <AutoColliders shape={"cuboid"}>
    <T.Mesh
      receiveShadow
      geometry={new BoxGeometry(10, 1, 10)}
      material={new MeshStandardMaterial()}
    />
  </AutoColliders>
</T.Group> -->
