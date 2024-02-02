<template>
  <div class="max-w-3xl ma">
    <h3 class="m-16px leading-8 text-6">{{ info?.data?.title }}</h3>
    <div
      v-for="i in info?.data.answers"
      class="
        relative
        mb-30px
        p-15px
        bg-#ececec
        overflow-hidden
        mx-16px
        rd-4
        cursor-pointer
      "
      :class="{ 'censor-item': i.censored }"
      @click="getInfo(i.answerID)"
    >
      <div
        class="line-height-8 line-clamp-5 text-justify max-h-50"
        v-html="i.content"
      ></div>
      <!-- <span class="color-#aaa font-size-3">{{ i.answerID }}</span> -->
      <div
        class="flex flex-justify-between flex-items-center mt-10px color-#999"
      >
        <div>
          <div>
            {{ itemFilter(i.excerpt).iptime }}
          </div>
        </div>

        <div :class="{ 'blur-3 hover:blur-0': i.censored }">
          <!-- <span class="vertical-68%"> </span> -->
          <a
            :href="'https://www.zhihu.com/people/' + i.author.urlToken"
            rel="noreferrer"
            target="_blank"
            class="flex items-center"
          >
            <span>{{ i.author.name }}</span>
            <img
              :src="i.author?.avatarURL"
              class="w-38px rounded ml-3"
              :alt="i.author.name"
            />
          </a>
        </div>
      </div>
    </div>
  </div>
  <AnswerDetail :answerID="answerID" />
</template>

<script setup>
import { ref } from 'vue';
const { params } = useRoute();

const sortby = ref('censored');
const { data, pending, error, refresh } = await useFetch(
  'https://api.mcdonald.workers.dev/questions/' + params.id,
  {
    query: { sortby, pagesize: 20, page: 1, sortorder: 'desc' },
  }
);
// console.log(data.value.data);
const info = ref(data);
const answerID = ref(null);
const itemFilter = (item) => {
  const regex =
    /(发布于|编辑于) (\d{4}-\d{2}-\d{2} \d{2}:\d{2})・IP 属地(.*?)(赞同|真诚)/;
  const match = regex.exec(item);
  console.log(match);
  if (match?.length < 5) return item;
  // const action = match[1];
  const time = match[2];
  const ip = match[3];
  const content = item.split(match[0])[0].trim();
  // console.log('时间 A:', time);
  // console.log('IP A:', ip);
  // console.log('内容 A:', content);
  console.log('---------------------');

  return { content, ip, time };
};

const getInfo = (id) => {
  console.log(id);
};
</script>
<style lang="scss" scoped>
@import 'https://static.zhihu.com/heifetz/main.216a26f4.636280215996da924864.css';
@import 'https://static.zhihu.com/heifetz/main-question-routes.216a26f4.76cd58bb46c08ca4ab3c.css';
.censor-item {
  text-indent: 1.5em;
  background: #eee;
  &:first-letter {
    font-size: 2em;
  }
  &:before {
    content: '已审查';
    position: absolute;
    left: -22px;
    top: 11px;
    transform: rotate(-45deg);
    font-size: 13px;
    color: #fff;
    background: #ffc107;
    padding: 0 20px;
    text-indent: 0;
  }
}
:deep {
  img {
    max-width: 100%;
    height: auto;
  }

  .ContentItem-actions {
    display: none;
  }
}
</style>
