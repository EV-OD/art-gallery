<script lang="ts">
  import { T, useFrame, useThrelte } from "@threlte/core";
  import { OrbitControls } from "@threlte/extras";
  import {
    Vector3,
    type PerspectiveCamera,
    Clock,
    Mesh,
    BoxGeometry,
    MeshStandardMaterial,
  } from "three";
  import { onMount } from "svelte";
  import { AutoColliders, CollisionGroups, Debug } from "@threlte/rapier";
  import { FirstPersonControls } from "three/addons/controls/FirstPersonControls.js";
  import { PointerLockControls } from "three/addons/controls/PointerLockControls.js";

  import Ground from "./ground.svelte";
  import { spring } from "svelte/motion";
  import Player from "./player.svelte";

  const { size, camera, renderer } = useThrelte();
  let fpControls: FirstPersonControls;
  let fpLockControls: PointerLockControls;

  let playerMesh: Mesh;
  let groundLoaded: boolean;

  let playerPosition: Parameters<Vector3["set"]> | undefined = [0, 10, 0];
  let pPos: Vector3 = new Vector3();
  let cameraRef: PerspectiveCamera | undefined;

  let camPosX: number;
  let camPosZ: number;

  onMount(() => {
    if (cameraRef) {
      fpLockControls = new PointerLockControls(cameraRef, document.body);
    }
    document.body.addEventListener("click", lockPointer);
    return () => {
      document.body.removeEventListener("click", lockPointer);
    };
  });

  function lockPointer() {
    fpLockControls.lock();
  }

  useFrame((ctx, delta) => {
    // fpControls.update(delta);
  });
</script>

<T.GridHelper args={[50]} />
<!-- <T.Group position.x={$smoothPlayerPosX} position.z={$smoothPlayerPosZ}> -->
<T.PerspectiveCamera bind:ref={cameraRef} makeDefault position={[0, 10, 0]}>
  <!-- <OrbitControls /> -->
</T.PerspectiveCamera>
<!-- </T.Group> -->

<Debug depthTest={false} depthWrite={false} />

<T.DirectionalLight position={[0, 10, 10]} />
<T.AmbientLight intensity={1} />
<CollisionGroups groups={[0, 15]}>
  <Ground bind:loaded={groundLoaded} />
  {#if groundLoaded}
    <Player bind:playerMesh bind:playerPosition />
  {/if}
</CollisionGroups>

<CollisionGroups groups={[0]} />
