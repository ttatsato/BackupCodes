<template>
  <div id="todo-item">
    <li>
       {{ title }}
       <button v-on:click=$emit(\'remove\')>X</button>
     </li>
    </div>
</template>
