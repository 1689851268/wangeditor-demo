<template>
    <div style="border: 1px solid #ccc">
        <Toolbar
            style="border-bottom: 1px solid #ccc"
            :mode="mode"
            :editor="editorRef"
            :defaultConfig="toolbarConfig"
        />
        <Editor
            style="height: 500px; overflow-y: hidden"
            v-model="valueHtml"
            :mode="mode"
            :defaultConfig="editorConfig"
            @onCreated="handleCreated"
        />
    </div>
</template>

<script lang="ts" setup>
import "@wangeditor/editor/dist/css/style.css"; // 引入 css

import { onBeforeUnmount, ref, shallowRef, onMounted } from "vue";
import { Editor, Toolbar } from "@wangeditor/editor-for-vue";

const mode = "default";

/* 配置工具栏 */
const editorRef = shallowRef(); // 编辑器实例; 必须用 shallowRef
const toolbarConfig = {};

/* 配置编辑器 */
const valueHtml = ref("<p>hello</p>"); // 内容 HTML
const editorConfig = { placeholder: "请输入内容..." };
const handleCreated = (editor: any) => {
    editorRef.value = editor; // 记录 editor 实例, 重要!
};

// 模拟 ajax 异步获取内容
onMounted(() => {
    setTimeout(() => {
        valueHtml.value = "<p>模拟 Ajax 异步设置内容</p>";
    }, 1500);
});

// 组件销毁时, 也及时销毁编辑器
onBeforeUnmount(() => {
    const editor = editorRef.value;
    if (editor == null) return;
    editor.destroy();
});
</script>
