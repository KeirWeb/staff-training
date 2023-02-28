<template>
  <div><h1>Добро пожаловать на курс</h1></div>
</template>

<script>
export default {
  data() {
    return {
      courses: null,
      api: null,
    };
  },
  mounted() {
    this.initApi();
    this.getCourses();
  },
  methods: {
    getCourses() {
      axios.get("/api/auth/courses").then((res) => {
        this.courses = res.data.data;
      });
    },
    initApi() {
      const api = axios.create();
      api.interceptors.request.use(
        (config) => {
          if (localStorage.getItem("access_token")) {
            config.headers = {
              autorization: `Bear ${localStorage.getItem("access_token")}`,
            };
          }
          return config;
        },
        (error) => {}
      );
      api.interceptors.response.use(
        (config) => {
          if (localStorage.getItem("access_token")) {
            config.headers = {
              autorization: `Bear ${localStorage.getItem("access_token")}`,
            };
          }
          return config;
        },
        (error) => {
          if (error.response.status === 401) {
            this.$router.push({ name: "user.login" });
          }
        }
      );
      this.api = api;
    },
  },
};
</script>

<style lang="scss"></style>
