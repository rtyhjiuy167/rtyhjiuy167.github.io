```
npm i axios
```



```javascript
import axios from "axios";

const service = axios.create({
  baseURL: import.meta.env.VITE_BASE_URL,
  timeout: 10000,
});
service.interceptors.request.use(
  (config) => {
    return config;
  },
  (error) => {
    console.log(error);
    Promise.reject(error);
  }
);

service.interceptors.response.use(
  (res) => {
    return res.data;
  },
  (error) => {
    console.log(error);
    return Promise.reject(new Error("error"));
  }
);
export default service;
```

