## Codigo

```js
<template>
  <div id="app">
    <q-btn
      class="draggable-btn"
      color="primary"
      label="Arrástrame"
      @mousedown="startDrag"
    />
  </div>
</template>

<script>
import { ref, onMounted, onBeforeUnmount } from "vue";

export default {
  setup() {
    const initialY = ref(0);
    const offsetY = ref(0);
    const isDragging = ref(false);
    const btnRef = ref(null);

    const startDrag = (event) => {
      isDragging.value = true;
      initialY.value = event.clientY - offsetY.value;
      document.addEventListener("mousemove", onDrag);
      document.addEventListener("mouseup", stopDrag);
    };

    const onDrag = (event) => {
      if (isDragging.value) {
        offsetY.value = event.clientY - initialY.value;
        if (btnRef.value) {
          btnRef.value.style.transform = `translateY(${offsetY.value}px)`;
        }
      }
    };

    const stopDrag = () => {
      isDragging.value = false;
      document.removeEventListener("mousemove", onDrag);
      document.removeEventListener("mouseup", stopDrag);
    };

    onMounted(() => {
      btnRef.value = document.querySelector(".draggable-btn");
    });

    onBeforeUnmount(() => {
      document.removeEventListener("mousemove", onDrag);
      document.removeEventListener("mouseup", stopDrag);
    });

    return {
      startDrag,
      btnRef,
    };
  },
};
</script>

<style>
.draggable-btn {
  position: fixed;
  top: 50%;
  left: 0;
  transform: translateY(-50%);
  cursor: grab;
}
.draggable-btn:active {
  cursor: grabbing;
}
</style>

```

Explicación:

Variables reactivas: Usamos ref para definir las variables initialY, offsetY, y isDragging.
Métodos:
  startDrag inicia el arrastre y agrega los listeners de mousemove y mouseup.
  onDrag actualiza la posición vertical del botón durante el arrastre.
  stopDrag detiene el arrastre y remueve los eventos.
Ciclo de vida:
  onMounted: Se ejecuta cuando el componente está montado, y usamos querySelector para obtener una referencia al botón.
  onBeforeUnmount: Se asegura de que los eventos se eliminen al desmontar el componente.
