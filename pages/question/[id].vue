<template>
  <div class="max-w-[min(50rem,80vw)] ma">
    <h3 class="p-16px leading-8 text-6 sticky top-0 z-1">
      {{ info?.data?.title }}
    </h3>
    <div
      v-for="i in info?.data.answers"
      class="list-item relative mb-30px p-15px bg-#eee overflow-hidden mx-16px rd-4 cursor-pointer"
      :class="{ 'censor-item': i.censored }"
      @click="getInfo(i.answerID)"
    >
      <div
        class="line-height-8 text-justify mx-3 pt-2"
        v-html="i.content"
      ></div>
      <!-- <span class="color-#aaa font-size-3">{{ i.answerID }}</span> -->
      <div
        class="flex flex-justify-between flex-items-center mt-10px color-#999"
      >
        <div>
          <div>
            <!-- {{ itemFilter(i.excerpt).iptime }} -->
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
//
const info = ref(data);
const answerID = ref(null);
const itemFilter = (item) => {
  const regex =
    /(发布于|编辑于) (\d{4}-\d{2}-\d{2} \d{2}:\d{2})・IP 属地(.*?)(赞同|真诚)/;
  const match = regex.exec(item);

  if (match?.length < 5) return item;
  // const action = match[1];
  const time = match[2];
  const ip = match[3];
  const content = item.split(match[0])[0].trim();
  //

  return { content, ip, time };
};

const getInfo = (id) => {};

const imgFilter = () => {
  // 检查所有的img，如果src为 data:image/svg+xml;utf8, 则替换为属性data-actualsrc
  const imgs = document.querySelectorAll(
    // ".RichContent img.origin_image.zh-lightbox-thumb.lazy",
    // ".RichContent img.content_image.lazy"
    '.RichContent figure img.lazy'
  );
  imgs.forEach((img) => {
    if (img.src.startsWith('data:image/svg+xml;')) {
      console.log('replace', img.getAttribute('data-actualsrc'));

      img.src = img.getAttribute('data-actualsrc');
    }
  });
};
onMounted(() => {
  imgFilter();
});
</script>
<style lang="scss" scoped>
@import 'https://static.zhihu.com/heifetz/main.216a26f4.636280215996da924864.css';
// @import "https://static.zhihu.com/heifetz/main-question-routes.216a26f4.76cd58bb46c08ca4ab3c.css";
.censor-item {
  // text-indent: 1.5em;
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
html[data-theme='dark'] .list-item {
  background: #191b1f;
}
:deep {
  figure img {
    width: 100%;
    height: auto;
  }

  .ContentItem-actions,
  .Reward {
    display: none;
  }
}
</style>
