在`plugins`下新建`globalloading.ts`，其内容如下：

```ts
import { createDiscreteApi } from 'naive-ui';
import { LoadingBarInst } from 'naive-ui/es/loading-bar/src/LoadingBarProvider';
export default defineNuxtPlugin(nuxtApp => {
  const bar = ref<LoadingBarInst>();
  nuxtApp.hook('app:mounted', () => {
    if (!bar.value) {
      const { loadingBar } = createDiscreteApi(['loadingBar']);
      bar.value = loadingBar;
    }
  });
  nuxtApp.hook('page:start', () => {
    bar.value?.start();
  });
  nuxtApp.hook('page:finish', () => {
    setTimeout(() => {
      bar.value?.finish();
    }, 150);
  });
  nuxtApp.hook('app:error', () => {
    if (process.client) {
      setTimeout(() => {
        bar.value?.finish();
      }, 150); // 速度过快延缓执行
    }
  });
});

```

