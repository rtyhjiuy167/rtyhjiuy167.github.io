### bean 实例化

### 使用无参构造函数实例化bean

如果待实例化的类没有写任何构造函数，会以无参构造函数进行实例化：

```java
public class BookDaoImpl implements BookDao {
    @Override
    public void save(){}
}
```

如果类中有有参构造函数，必须手动添加无参构造函数，否则无法进行实例化：

```java
public class BookDaoImpl implements BookDao {
    public BookDaoImpl(int i){

    }
    public BookDaoImpl(){

    }
    @Override
    public void save(){}
}
```

### 工厂函数实例化bean

bean标签里使用`factory-method`属性来配置类中用来返回实例化对象的方法名

### 使用实例工厂实例化bean

基本不用

### 使用FactoryBean实例化bean

使用FactoryBean实例化bean为使用实例工厂实例化bean的改良。

```java
package com.rtyhjiuy15.factory;

import com.rtyhjiuy15.dao.BookDao;
import com.rtyhjiuy15.dao.impl.BookDaoImpl;
import org.springframework.beans.factory.FactoryBean;

public class BookDaoFactoryBean implements FactoryBean<BookDao> {

    @Override
    public BookDao getObject() throws Exception {
        return new BookDaoImpl();
    }

    @Override
    public Class<?> getObjectType() {
        return BookDao.class;
    }

    // isSingleton 用来配置是否为单例，默认为 true
    @Override
    public boolean isSingleton() {
        return true;
    }
}
```

```xml
<bean id="BookDaoImpl" class="com.rtyhjiuy15.factory.BookDaoFactoryBean"/>
```

