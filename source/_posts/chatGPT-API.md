---
title: chatGPTAPI使用Demo(Vue)
tags:
  - chatGPT
  - vue
  - TailwindCss
categories:
  - Vue
abbrlink: 90236ad6
date: 2023-02-12 14:29:01
---

- API文档地址:  [API Reference - OpenAI API](https://platform.openai.com/docs/api-reference/introduction)
- 在线演示: https://platform.openai.com/playground
- 微软:  https://www.bing.com/new
- 缺点: 准确度还并不是很高，未实现类似一个字一个字的生成，若返回的内容过多，可能需要很久
- 这个md渲染不支持Vue...

```vue
<template>
    <div class="mx-auto bg-white rounded-xl shadow-md items-center w-96">
        <h1 class=" text-red-500 mx-auto w-20">chat GPT</h1>
        <input class="w-80" type="text" @keydown.enter="sendRequest" autofocus placeholder="请输入一个问题">
        <div>
            <ul>
                <template v-for="dialogue of dialogues">
                    <li>Human: {{ dialogue.ask }}</li>
                    <li>AI: {{ dialogue.answer }}</li>
                </template>
            </ul>
        </div>
    </div>
</template>

<script setup>
import { Configuration, OpenAIApi } from "openai"
import { reactive } from "vue";
const configuration = new Configuration({
    organization: "orgination ID",
    apiKey: "APP key",
});
const openai = new OpenAIApi(configuration);
const dialogues = reactive([]);
// openai.listModels().then((response) => {
//     console.log(response);
// });
function sendRequest(e) {
    const mes = e.target.value;
    e.target.value = "";
    dialogues.push({
        ask: mes,
        answer: "loading...",
    });
    // https://platform.openai.com/docs/api-reference/introduction
    openai.createCompletion({
        model: "text-davinci-003",
        prompt: mes,
        // max_tokens: 7,
        // 随机性0~2
        // temperature: 0,
        // logprobs: 5,
        // 是否显示自己所输入的
        // echo: true,
        temperature: 0.9,
        max_tokens: 150,
        top_p: 1,
        frequency_penalty: 0,
        presence_penalty: 0.6,
        // stop: [" Human:", " AI:"],
    }).then((response) => {
        console.log(response);
        dialogues[dialogues.length - 1].answer = response.data.choices[0].text;
    });
}
```

> 演示: https://v.douyin.com/BvSFDED/ 