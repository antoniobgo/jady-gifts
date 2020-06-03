<template lang="pug">
  v-app
    v-app-bar(app)
      v-row(justify="center")
        v-toolbar-title Jadynha e seus presentes
    v-content(fluid)
      v-container
        router-view
</template>

<script>
export default {
  name: "App",

  data: () => ({
    //
  }),
};
</script>
