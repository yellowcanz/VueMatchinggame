<template>
  <li>
    <button>{{ isShow ? title : cover }}</button>
  </li>
</template>

<script>
export default {
  props: {
    title: String,
    cover: String,
    isShow: Boolean,
  },
}
</script>

<style scope>
button {
  display: flex;
  width: inherit;
  height: inherit;
  border-radius: 0.1rem;
  align-items: center;
  justify-content: center;
  outline: none;
  padding: 0;
  border: 0;
  background: linear-gradient(135deg, rgba(230, 230, 230, 1) 0%, rgba(246, 246, 246, 1) 100%);
  box-shadow: -4px -4px 10px -8px rgba(255, 255, 255, 1), 4px 4px 10px -8px rgba(0, 0, 0, 0.3);
  font-size: inherit;
}
button:active {
  box-shadow: -4px -4px 10px -8px rgba(255, 255, 255, 1) inset, 4px 4px 10px -8px rgba(0, 0, 0, 0.3) inset;
}
.gameBoard .Four {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 48px;
  height: 48px;
  margin: 0.2rem;
  font-size: 0.28rem;
}
.gameBoard .Eight {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 34px;
  height: 34px;
  margin: 0.1rem;
  font-size: 0.2rem;
}
</style>
