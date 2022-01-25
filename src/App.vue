<template>
  <div id="app">
    <div
      class="app-icon-container"
      style="
        align-items: flex-start;
        display: flex;
        width: 300px;
        height: 1024px;
        flex-flow: column wrap;
        align-content: flex-start;
      "
      >
    <!-- 
      <AppIconItem
        v-for="item in app_list"
        :key="item"
        :app_name="item.name"
        :app_path="item.path"
        :icon_src="item.icon"
      ></AppIconItem> -->
    </div>

    <button @click="Scanapp">扫描桌面快捷方式</button>
    <button @click="Exit">EXIT</button>
    <div class="widget-container">
      <WidgetCard :size="mid"><Weather></Weather></WidgetCard>
      <WidgetCard
        :size="min"
        :cardbg="'url(imgs/clockbg.png)'"
        :bgcolor="'rgb(0, 122, 204)'"
        ><Clock></Clock
      ></WidgetCard>
      <WidgetCard
        :size="min"
        :cardbg="dd.bg"
        v-for="dd in deadlines"
        :key="dd"
        :bgcolor="dd.bgcolor"
      >
        <Countdown
          :target="dd.target"
          :name="dd.name"
          :cl_title="dd.colors[0]"
          :cl_num="dd.colors[1]"
          :cl_target="dd.colors[2]"
        ></Countdown>
      </WidgetCard>

      <WidgetCard :size="mid"><Calendar></Calendar></WidgetCard>

      <WidgetCard :size="max" :bgcolor="'#f5f8fd'"
        ><class-schedule></class-schedule
      ></WidgetCard>
    </div>
    <div style="position: absolute; color: white; right: 10px; bottom: 10px">
      Develop version build 2501
    </div>
  </div>
</template>
<script>
// import AppIconItem from "./components/AppIconItem.vue";
import WidgetCard from "./components/WidgetCard.vue";
import ClassSchedule from "./components/widgets/ClassSchedule.vue";
import applists from "./configs/lists.json";
// 自定义组件

export default {
  publicPath: "./assets",
  name: "App",
  components: {
    WidgetCard,
    // AppIconItem,
    ClassSchedule,
    // Calendar,
    // Weather,Clock,Countdown,
  },
  data: function () {
    return {
      min: "min",
      max: "max",
      mid: "mid",
      app_list: [],
      deadlines: [
        {
          name: "钟离、甘雨UP",
          target: "2022-1-25",
          style1: "color:white;",
          colors: ["black", "rgb(204, 22, 58)", "gray"],
          bg: "url(imgs/count_tmp.png)",
        },
        {
          name: "开学",
          target: "2022-2-18",
          colors: [],
        },
      ],
    };
  },
  methods: {
    Scanapp: function () {
      var data = {
        method: "scanapp",
      };
      window.chrome.webview.postMessage(JSON.stringify(data));
    },
    Exit: function () {
      var data = {
        method: "exit",
      };
      window.chrome.webview.postMessage(JSON.stringify(data));
    },
  },
  mounted: function () {
    this.app_list = applists;
  },
};
</script>

<style>
:root {
  --widget-margin: 15px;
  --icon-size: 56px;
  --icon-gap-x: 35px;
  --icon-gap-y: 35px;
  --icon-border-radius: 15px;
}
html {
  background-image: url("assets/wp.png");
  width: 100%;
  height: 100vh;
  margin: 0;
  padding: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #1a2c3f;

  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  align-items: flex-start;
}
.widget-container {
  width: 40%;
  height: 100%;
  display: grid;
  height: 100%;
  grid-template-columns: repeat(
    auto-fill,
    calc(var(--icon-size) + var(--icon-gap-y))
  );
  grid-template-rows: repeat(
    auto-fill,
    calc(var(--icon-size) + var(--icon-gap-x))
  );
  grid-auto-flow: dense;
  box-sizing: border-box;
  justify-content: center;
  /* backdrop-filter: blur(5px); */
}
</style>
