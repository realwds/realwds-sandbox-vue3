<template>
  <el-tabs v-model="activeName" type="border-card" @tab-click="handleClick">
    <el-tab-pane label="Home" name="home">
      <HomeView />
    </el-tab-pane>
    <el-tab-pane label="About" name="about">
      <AboutView />
    </el-tab-pane>
    <el-tab-pane label="Draggable" name="draggable">
      <Draggable />
    </el-tab-pane>
  </el-tabs>
</template>

<script setup>
import { ref } from "vue";
import HomeView from "./views/HomeView.vue";
import AboutView from "./views/AboutView.vue";
import Draggable from "./views/Draggable.vue";

const activeName = ref("draggable");
const handleClick = function (e) {
  console.log(e);
};
</script>
