---
title: Vue Hooks
published: 2023-11-13
description: 'Vue 3 Composition API Hooks 教程'
image: ''
tags: [Vue]
category: 'Vue'
draft: false 
---

# Vue 3 Composition API Hooks 教程

Vue 3 引入了 Composition API，使得我们可以用更灵活、更模块化的方式来组织代码。在开发过程中，我们常常会遇到多个组件需要复用逻辑的场景，这时自定义 Hook （即自定义组合函数）成为了一个非常强大的工具。

本篇博客将从以下几个方面教你如何编写 Vue 自定义 Hook：

1. 什么是 Vue Hook？
2. 自定义 Hook 的使用场景
3. 如何编写一个简单的 Hook
4. 实际案例：编写常见的 Hook
5. Hook 的优化与注意事项

---

## 1. 什么是 Vue Hook？

Vue Hook 是指基于 Composition API 的自定义组合函数（`function`）。它允许我们提取组件中可复用的逻辑，并通过调用这些函数来实现逻辑的共享。

自定义 Hook 通常以 `use` 开头，例如：`useFetch`、`useCounter`、`useMouse`，以便开发者明确这是一个可复用逻辑的函数。

---

## 2. 自定义 Hook 的使用场景

以下是自定义 Hook 的常见使用场景：

1. **状态管理**：多个组件需要共享的状态。
2. **数据获取**：封装通用的接口请求逻辑。
3. **事件监听**：如鼠标位置、窗口尺寸变化等。
4. **复杂逻辑复用**：如表单校验、分页逻辑等。

---

## 3. 如何编写一个简单的 Hook

编写hook前我们需要理解几个核心概念：

- 响应式数据（`ref` 和 `reactive`）
- 生命周期函数（如 `onMounted`, `onUnmounted`）
- 计算属性（`computed`）
- 侦听器（`watch` 和 `watchEffect`）

逻辑复用： 当多个组件需要共享逻辑时，把逻辑提取到普通函数中。

理解 hook 的优势：把普通函数转换为“可感知 Vue 环境”的自定义 Hook，也就是说hook和状态密切关联，甚至可以使用声明周期函数管理逻辑

### 示例：一个计数器 Hook

我们先来编写一个简单的计数器 Hook，它包含增加、减少、重置功能。

```javascript
// hooks/useCounter.js
import { ref } from 'vue';

export function useCounter(initialValue = 0) {
  const count = ref(initialValue);

  const increment = () => {
    count.value++;
  };

  const decrement = () => {
    count.value--;
  };

  const reset = () => {
    count.value = initialValue;
  };

  return {
    count,
    increment,
    decrement,
    reset,
  };
}
```

### 在组件中使用

```vue
<script setup>
import { useCounter } from './hooks/useCounter';

const { count, increment, decrement, reset } = useCounter(10);
</script>

<template>
  <div>
    <p>当前计数：{{ count }}</p>
    <button @click="increment">增加</button>
    <button @click="decrement">减少</button>
    <button @click="reset">重置</button>
  </div>
</template>
```

### 示例2：数据获取 Hook

当我们需要从 API 获取数据时，可以封装一个通用的 `useFetch` Hook。

```javascript
// hooks/useFetch.js
import { ref, onMounted } from 'vue';

export function useFetch(url) {
  const data = ref(null);
  const error = ref(null);
  const loading = ref(false);

  const fetchData = async () => {
    loading.value = true;
    try {
      const response = await fetch(url);
      data.value = await response.json();
    } catch (err) {
      error.value = err;
    } finally {
      loading.value = false;
    }
  };

  onMounted(fetchData);

  return {
    data,
    error,
    loading,
    fetchData,
  };
}
```

### 在组件中使用

```vue
<script setup>
import { useFetch } from './hooks/useFetch';

const { data, error, loading } = useFetch('https://api.example.com/items');
</script>

<template>
  <div>
    <p v-if="loading">加载中...</p>
    <p v-if="error">加载失败：{{ error.message }}</p>
    <ul v-if="data">
      <li v-for="item in data" :key="item.id">{{ item.name }}</li>
    </ul>
  </div>
</template>
```

### 示例3：窗口尺寸 Hook

```javascript
// hooks/useWindowSize.js
import { ref, onMounted, onUnmounted } from 'vue';

export function useWindowSize() {
  const width = ref(window.innerWidth);
  const height = ref(window.innerHeight);

  const updateSize = () => {
    width.value = window.innerWidth;
    height.value = window.innerHeight;
  };

  onMounted(() => {
    window.addEventListener('resize', updateSize);
  });

  onUnmounted(() => {
    window.removeEventListener('resize', updateSize);
  });

  return { width, height };
}
```

### 在组件中使用

```vue
<script setup>
import { useWindowSize } from './hooks/useWindowSize';

const { width, height } = useWindowSize();
</script>

<template>
  <div>
    <p>窗口宽度：{{ width }}</p>
    <p>窗口高度：{{ height }}</p>
  </div>
</template>
```

---

## 5. Hook 的优化与注意事项

1. **命名规范**：自定义 Hook 应以 `use` 开头，便于识别。
2. **解耦逻辑**：Hook 只负责逻辑处理，不涉及 UI 渲染。
3. **性能优化**：避免不必要的依赖，合理使用 `watch` 和 `onUnmounted`。
4. **避免重复执行**：对于需要全局唯一的逻辑（如事件监听），可使用单例模式。

## 6. 阅读成熟的 Hooks 源码，尝试模仿和改进。例如：

- 从开源库如 [VueUse](https://vueuse.org/) 中挑选简单的 Hook，分析其代码。
- 在自己的项目中复现它的功能，逐步优化。

---

通过自定义 Hook，我们可以更好地组织代码，提升代码复用性和可维护性。希望这篇教程能帮助你掌握 Vue 3 Hook 的使用！