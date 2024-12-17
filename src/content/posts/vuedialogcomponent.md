---
title: vue3封装弹框组件
published: 2023-8-17
description: '弹框组件封装思路，结合vue特性如何优雅的封装弹框'
image: ''
tags: [组件封装]
category: '组件封装'
draft: false 
---

在开发后台系统时，我们经常用到弹框组件，如表单填写，当然我们可以直接将dialog组件相关逻辑都写在父组件中，但是呢，这样会不太优雅，组件的复杂的话会导致页面可读性差以及代码管理混乱。接下来将介绍几种弹框封装思路

### 思路一：

在子组件里使用响应式来管理弹框的visible显隐状态，通过defineExpose来暴露子组件方法，使得父组件可以控制子组件的显隐。

子组件
```js
// ChildDialog.vue
<template>
  <div class="container">
    <el-dialog v-model="visible"></el-dialog>
  </div>
</template>

<script setup>
import { ref } from "vue";

const visible = ref(false);

const open = () => {
  visible.value = true;
};

const close = () => {
  visible.value = false;
};

defineExpose({
  open,
  close,
});
</script>
```
父组件

```js
<template>
  <ChildDialog ref="dialogRef" />
  <el-button @click="openDialog">打开弹框</el-button>
</template>

<script setup>
import ChildDialog from "./ChildDialog.vue";
import { ref } from "vue";
const dialogRef = ref(false);
const openDialog = () => {
  dialogRef.value.open();
};
const closeDialog = () => {
  dialogRef.value.close();
};
</script>
```

### 思路二：

通过父组件传递props，子组件监听props下visible来更新本地的dialogVisible进行显隐控制，子组件定义close关闭方法，通过该方法触发父组件close方法设置dialogVisible为false，弹窗关闭

子组件
```js
// ChildDialog.vue
<template>
  <el-dialog v-model="dialogVisible" @close="close" />
</template>

<script setup>
import { ref, watch } from "vue";

const props = defineProps({
  visible: {
    type: Boolean,
    default: false,
  },
});

const dialogVisible = ref(false);

watch(
  () => props.visible,
  (val) => {
    dialogVisible.value = val;
  }
);

const emits = defineEmits(["close"]);
const close = () => {
  emits("close");
};
</script>

```
父组件

```js
<template>
  <div>
    <Child :visible="visible" @close="close" />
    <button @click="visible = true">打开弹框</button>
  </div>
</template>

<script setup>
import ChildDialog from "./ChildDialog.vue";
import { ref } from "vue";

const visible = ref(false);

const close = () => {
  visible.value = false;
};
</script>

```

### 思路三：

为了减少父组件的多余代码，基于v-model来进行改造，父组件使用v-model传递默认modelValue给子组件，子组件通过计算属性获取到响应式数据visible，子组件close关闭事件会修改visible，从而触发update:modelValue方法，父组件修改本地数据，传递给子组件的数据也跟住变化

> `v-model` 在子组件上实际上是一个语法糖，它相当于传递了一个名为 `modelValue` 的 prop，并且监听一个名为 `update:modelValue` 的事件。

子组件
```js
// ChildDialog.vue
<template>
  <el-dialog v-model="visible" />
</template>

<script setup>
import { computed } from "vue";

const props = defineProps({
  modelValue: {
    type: Boolean,
    default: false,
  },
});

const emits = defineEmits(["update:modelValue"]);
const visible = computed({
  get: () => props.modelValue,
  set: (val) => {
    emits("update:modelValue", val);
  },
});
</script>

```
父组件

```js
<template>
  <div>
    <ChildDialog v-model="visible" />
    <el-button @click="visible = true">打开弹窗</el-button>
  </div>
</template>

<script setup>
import { ref } from "vue";
import ChildDialog from "./ChildDialog.vue";
const visible = ref(false);
</script>

```

下一期将介绍如何写一个命令式弹框
