#### TS

在`src/plugins/`目录下，新建`cache.ts`文件，其内容如下：

```ts
const sessionCache = {
  set(key: string, value: string) {
    if (!sessionStorage) {
      return;
    }
    sessionStorage.setItem(key, value);
  },
  get(key: string) {
    if (!sessionStorage) {
      return null;
    }
    return sessionStorage.getItem(key);
  },
  setJSON(key: string, jsonValue: unknown) {
    if (jsonValue != null) {
      this.set(key, JSON.stringify(jsonValue));
    }
  },
  getJSON(key: string) {
    const value = this.get(key);
    if (value != null) {
      return JSON.parse(value);
    }
  },
  remove(key: string) {
    sessionStorage.removeItem(key);
  },
};
const localCache = {
  set(key: string, value: string) {
    if (!localStorage) {
      return;
    }
    localStorage.setItem(key, value);
  },
  get(key: string) {
    if (!localStorage) {
      return null;
    }
    return localStorage.getItem(key);
  },
  setJSON(key: string, jsonValue: unknown) {
    if (jsonValue != null) {
      this.set(key, JSON.stringify(jsonValue));
    }
  },
  getJSON(key: string) {
    const value = this.get(key);
    if (value != null) {
      return JSON.parse(value);
    }
  },
  remove(key: string) {
    localStorage.removeItem(key);
  },
};

export default {
  session: sessionCache,
  local: localCache,
};
```

#### JS

在`src/plugins/`目录下，新建`cache.ts`文件，其内容如下：

```ts
const sessionCache = {
  set(key, value) {
    if (!sessionStorage) {
      return;
    }
    if (key != null && value != null) {
      sessionStorage.setItem(key, value);
    }
  },
  get(key) {
    if (!sessionStorage) {
      return null;
    }
    if (key == null) {
      return null;
    }
    return sessionStorage.getItem(key);
  },
  setJSON(key, jsonValue) {
    if (jsonValue != null) {
      this.set(key, JSON.stringify(jsonValue));
    }
  },
  getJSON(key) {
    const value = this.get(key);
    if (value != null) {
      return JSON.parse(value);
    }
  },
  remove(key) {
    sessionStorage.removeItem(key);
  },
};
const localCache = {
  set(key, value) {
    if (!localStorage) {
      return;
    }
    if (key != null && value != null) {
      localStorage.setItem(key, value);
    }
  },
  get(key) {
    if (!localStorage) {
      return null;
    }
    if (key == null) {
      return null;
    }
    return localStorage.getItem(key);
  },
  setJSON(key, jsonValue) {
    if (jsonValue != null) {
      this.set(key, JSON.stringify(jsonValue));
    }
  },
  getJSON(key) {
    const value = this.get(key);
    if (value != null) {
      return JSON.parse(value);
    }
  },
  remove(key) {
    localStorage.removeItem(key);
  },
};

export default {
  session: sessionCache,
  local: localCache,
};
```

#### js-cookie

如果需要考虑过期时间，则可以使用第三方库`js-cookie`。

```bash
npm i js-cookie
```

```bash
# ts 中使用需要安装类型
npm i --save-dev @types/js-cookie
```

