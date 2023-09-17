## 操作 mysql

操作 mysql 可以使用 `egg-mysql`或`egg-sequelize`。

### egg-mysql

```bash
npm i egg-mysql
```

在`config/plugin.js`中进行配置：

```js
module.exports = {
  mysql: {
    enable: true,
    package: 'egg-mysql',
  },
};

```

在`config/config.default.js`中配置：

```js
exports.mysql = {
    client: {
        host: 'localhost',
        port: '3306',
        user: 'root',
        password: '123456',
        database: '数据库名',
    },
    app: true,
    agent: false,
};
```

注意：mysql8之前的版本中加密规则是mysql_native_password，mysql8之后,加密规则是caching_sha2_password，而`egg-mysql`底层没有进行支持，需要进行相应修改：

```sql
mysql -h localhost -P 3306 -u root -p '123456
```

```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY '123456' PASSWORD EXPIRE NEVER;
```

```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456';
```

```sql
FLUSH PRIVILEGES; 
```

#### 数据操作

查询一条数据：

```js
const res = await this.app.mysql.get('sys_user', { user_id: 1 });
```

查找符合条件的数据：

```js
const res = await this.app.mysql.select('sys_user', {
    limit: 10,
    orders: [[ 'id', 'desc' ]],
});
```

插入数据：

```js
const res = await this.app.mysql.insert('sys_user', { user_name: 'hhh', nick_name: '小明' });
```

删除数据：

```js
const res = await this.app.mysql.delete('sys_user', { user_id: 118 });
```

原生sql：

```js
const res = await this.app.mysql.query('update sys_user set nick_name=? where user_id=?', [ '???', 118 ]);
```

#### 事务

```js
const conn = await this.app.mysql.beginTransaction();
try {
    await conn.delete('sys_user', { user_id: 118 });
    await conn.commit();
} catch (err) {
    await conn.rollback();
    throw err;
}
```

### egg-sequelize

```bash
npm i egg-sequelize mysql2
```

在`config/plugin.js`中进行配置：

```js
module.exports = {
  sequelize: {
    enable: true,
    package: 'egg-sequelize',
  },
};
```

在`config/config.default.js`中配置：

```js
config.sequelize = {
    dialect: "mysql",
    database: "center-website",
    host: "localhost",
    port: "3306",
    username: "root",
    password: "123456",
};
```

创建`app/model`文件夹，在`model`目录中创建`user.js`：

```js
module.exports = app => {
  const { STRING, INTEGER, DATE } = app.Sequelize;

  const User = app.model.define('user', {
    login: STRING,
    name: STRING(30),
    password: STRING(32),
    age: INTEGER,
    last_sign_in_at: DATE,
    created_at: DATE,
    updated_at: DATE,
  });

  return User;
};
```

#### 关联操作

##### 一对一

`app/model/organization.js`：

```js
module.exports = app => {
  const { INTEGER } = app.Sequelize;

  const Organization = app.model.define(
    'organizations', {
      branchId: {
        type: INTEGER,
        primaryKey: true, // 主键
        autoIncrement: true, // 主键自增
        comment: '部门ID ',
      },
    }
  );
  Organization.associate = function() {
    app.model.Organization.belongsTo(app.model.User, {
      foreignKey: 'uid',
      onDelete: 'CASCADE',
      onUpdate: 'CASCADE',
    });
  };
  Organization.sync({ force: true }).then(async () => {});
  return Organization;
};
```

`app/model/user.js`：

```js
module.exports = app => {
  const { STRING, INTEGER } = app.Sequelize;

  const User = app.model.define('user', {
    uid: {
      type: INTEGER,
      allowNull: false,
      primaryKey: true,
      autoIncrement: true,
    },
    password: STRING(32),
  });
  User.associate = function() {
    app.model.User.hasOne(app.model.User, {
      foreignKey: 'uid',
      onDelete: 'CASCADE',
      onUpdate: 'CASCADE',
    });
  };
  return User;
};
```

##### 多对多

`app/model/user.js`：

```js
module.exports = (app) => {
  const { STRING, INTEGER } = app.Sequelize;
  const User = app.model.define(
    "user",
    {
      uid: {
        type: INTEGER,
        allowNull: false,
        primaryKey: true,
        autoIncrement: true,
      },
      password: STRING(32),
    },
    {
      //强制表名称等于模型名称
      freezeTableName: true,
      // 是否需要 createdAt 与 updatedAt，默认为 true
      timestamps: false,
    }
  );

  User.associate = function () {
    User.associate = function () {
      app.model.User.belongsToMany(app.model.Role, {
        through: app.model.UserRole,
        foreignKey: "uid",
      });
    };
  };
  User.sync({ force: true }).then(async () => {});
  return User;
};
```

`app/model/role.js`：

```javascript
module.exports = (app) => {
  const { STRING, INTEGER } = app.Sequelize;
  const Role = app.model.define(
    "roles",
    {
      roleId: {
        type: INTEGER,
        allowNull: false,
        primaryKey: true,
        autoIncrement: true,
      },
      roleName: {
        type: STRING,
        allowNull: false,
        comment: "用户名",
      },
    },
    {
      //强制表名称等于模型名称
      freezeTableName: true,
      // 是否需要 createdAt 与 updatedAt，默认为 true
      timestamps: false,
    }
  );
  Role.associate = function () {
    app.model.Role.belongsToMany(app.model.User, {
      through: app.model.UserRole,
      foreignKey: "roleId",
    });
  };
  Role.sync({ force: true }).then(async () => {});
  return Role;
};

```

`app/model/user_role.js`：

```javascript
module.exports = (app) => {
  const { INTEGER } = app.Sequelize;
  const UserRole = app.model.define(
    "user_roles",
    {
      // 没有指定主键仍会生成 id 作为主键
      uid: {
        type: INTEGER,
        references: {
          model: app.model.User,
          key: "uid", // 指定 User 中的 uid 作为键
        },
        onDelete: "CASCADE", // User 表中进行删除时 UserRoles里也进行删除
        onUpdate: "CASCADE", // 级联删除
      },
      roleId: {
        type: INTEGER,
        references: {
          model: app.model.Role,
          key: "roleId",
        },
        onDelete: "CASCADE",
        onUpdate: "CASCADE",
      },
    },
    {
      //强制表名称等于模型名称
      freezeTableName: true,
      // 是否需要 createdAt 与 updatedAt，默认为 true
      timestamps: false,
    }
  );
  return UserRole;
};

```

