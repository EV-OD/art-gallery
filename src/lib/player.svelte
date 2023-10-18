<script lang="ts">
  import type { RigidBody as RapierRigidBody } from "@dimforge/rapier3d-compat";
  import { T, useFrame, useThrelte } from "@threlte/core";
  import {
    AutoColliders,
    BasicPlayerController,
    RigidBody,
  } from "@threlte/rapier";
  import { onMount } from "svelte";
  import {
    BoxGeometry,
    CapsuleGeometry,
    Mesh,
    MeshStandardMaterial,
    Vector3,
  } from "three";

  export let playerMesh: Mesh;

  export let playerPosition: Parameters<Vector3["set"]> | undefined = [
    0, 10, 0,
  ];
  let rigidBody: RapierRigidBody;
  const playerPos = new Vector3();
  const cameraDirection = new Vector3();

  let { camera } = useThrelte();

  onMount(() => {
    document.addEventListener("keydown", handleMove);
    return () => {
      document.removeEventListener("keydown", handleMove);
    };
  });

  function handleMove(e: KeyboardEvent) {
    if (playerMesh && playerPosition && rigidBody) {
      rigidBody.lockRotations(true, true);

      let speed = 1;

      let v3 = rigidBody.translation();

      playerPos.x = v3.x;
      playerPos.y = v3.y;
      playerPos.z = v3.z;

      $camera.getWorldDirection(cameraDirection);
      if (e.key == "s") {
        speed *= -1;
      }

      let diff = cameraDirection.clone().multiplyScalar(speed);
      diff.y = 0;
      let newPos = playerPos.clone().add(diff);
      rigidBody.setTranslation(newPos, true);
      v3 = rigidBody.translation();
      $camera.position.set(v3.x, v3.y + 10, v3.z);
    }
  }
</script>

<T.Group position={[0, 4, 0]}>
  <RigidBody bind:rigidBody type={"dynamic"} canSleep={true}>
    <AutoColliders shape={"cuboid"} friction={0.0} restitution={0} mass={0}>
      <T.Mesh
        bind:ref={playerMesh}
        receiveShadow
        castShadow
        geometry={new CapsuleGeometry(0.3, 1.8 - 0.3 * 2)}
        material={new MeshStandardMaterial()}
      />
    </AutoColliders>
  </RigidBody>
</T.Group>
