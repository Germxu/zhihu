<template>
  <ul ref="observerEl">
    <li v-for="i in list.data">
      <NuxtLink
        :to="'/question/' + i.id"
        class="flex align-center justify-between"
      >
        <div>
          <p class="text-14px">{{ i.title }}</p>
          <span class="text-13px">
            {{ i.censoredTimeOrAnswerCensoredUpdated }}
          </span>
        </div>
        <div class="text-#999 text-12px flex flex-col truncate text-center">
          <img :src="i.authorAvatarURL" alt="avatar" />
          <span>
            {{ i.authorName }}
          </span>
        </div>
      </NuxtLink>
    </li>
  </ul>
</template>

<script setup>
import "~/assets/css/index.css";
import { ref } from "vue";
const appConfig = useAppConfig();
const route = useRoute();

console.log("appConfig", appConfig);
console.log("route", route);

const page = ref(1);
const sortby = ref("censored_time_or_answer_censored_updated");
const { data } = await useFetch("https://api.mcdonald.workers.dev/questions", {
  query: {
    page,
    sortby,
    pagesize: 30,
    sortorder: "desc",
  },
});
const list = ref(data);

// 检测滚动到页面底部，加载更多
</script>
<style lang="scss" scoped>
ul {
  margin: 30px auto;
  max-width: min(50rem, 80vw);
  padding: 0;
  list-style: none;
  li {
    margin-bottom: 10px;
    // display: flex;
    // flex-wrap: wrap;
    padding-bottom: 20px;
    border-bottom: 1px solid #ececec;
    cursor: pointer;
    span {
      color: #999;
    }
    img {
      width: 38px;
      border-radius: 10px;
    }
  }
}
</style>
