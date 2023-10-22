<template>
  <div class="max-w-3xl ma">
    <h3 class="m-16px">{{ info?.data?.title }}</h3>
    <div
      v-for="i in info?.data.answers"
      class="mb-30px p-15px bg-#eee9 rd overflow-hidden mx-16px cursor-pointer"
      :class="{ 'shadow-[0_-3.5px_0_-1px_#f00] pt-12px': i.censored }"
      @click="getInfo(i.answerID)"
    >
      <div class="line-height-8 line-clamp-5 text-justify">
        {{ itemFilter(i.excerpt).content }}
      </div>
      <!-- <span class="color-#aaa font-size-3">{{ i.answerID }}</span> -->
      <div
        class="flex flex-justify-between flex-items-center mt-20px color-#999"
      >
        <div>
          <div>
            {{ itemFilter(i.excerpt).iptime }}
          </div>
        </div>

        <div>
          <!-- <span class="vertical-68%"> </span> -->
          <a
            :href="'https://www.zhihu.com/people/' + i.author.urlToken"
            rel="noreferrer"
            target="_blank"
          >
            {{ i.author.name }}
            <img :src="i.author?.avatarURL" class="w-38px rounded ml-3" />
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
