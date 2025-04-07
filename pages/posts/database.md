---
title: 数据库相关
date: 2025-04-07
lang: zh
duration: 10min
type: note
---

[[TOC]]

## 事务的特性（ACID）

```shell
A（原子性）	一组操作要么全做要么全不做
C（一致性）	事务执行前后，数据必须满足所有约束
I（隔离性）	并发事务之间互不干扰
D（持久性）	一旦提交，数据永久生效
```

## 事务的隔离级别（Isolation Levels）
| 隔离级别                 | 脏读 | 不可重复读 | 幻读               |
|--------------------------|------|------------|--------------------|
| 读未提交 (Read Uncommitted) | ✅    | ✅          | ✅                 |
| 读已提交 (Read Committed)   | ❌    | ✅          | ✅                 |
| 可重复读 (Repeatable Read)  | ❌    | ❌          | ✅（MySQL 实际防了） |
| 串行化 (Serializable)      | ❌    | ❌          | ❌                 |
✅ MySQL 默认使用 REPEATABLE READ，还能通过“间隙锁”解决幻读。

## 三种典型并发问题对比
| 问题类型     | 发生条件         | 描述                     | 示例                                |
|--------------|------------------|--------------------------|-------------------------------------|
| 脏读         | 读到未提交数据   | 数据可能是无效的         | A读取了B尚未提交的数据             |
| 不可重复读   | 同一行被修改     | 两次读同一行，值不同     | A读取了两次 balance，B 中间改了它 |
| 幻读         | 记录集合变了     | 两次查集合，条数不同     | A查余额 >1000 的记录，B中间插入了符合条件的新行 |


区别重点：


不可重复读关注：同一行数据前后不一致

幻读关注：集合大小前后不一致（行数多了/少了）

## 为什么要区分不可重复读和幻读？
数据库需要用不同的锁策略解决它们：

不可重复读 → 用行锁

幻读 → 用间隙锁 / Next-Key Lock

精确划分问题，能提高并发性能降低锁粒度避免不必要的阻塞
本质：让数据库既保证数据一致性，又不牺牲太多性能


## MyBatis

MyBatis 在 Mapper 接口中**接收参数**，可以支持以下几种类型，我们可以分为 **6类常见方式**：

---

## ✅ 1. **单个简单类型参数**

- 适用于：只有一个参数，类型是基本数据类型或其包装类（如 `int`, `String`, `Integer`, `Long` 等）

```java
User selectById(int id);
```

```xml
SELECT * FROM user WHERE id = #{id}
```

✅ **参数名 = 方法的参数名 or param1/arg0**

---

## ✅ 2. **多个简单类型参数**

如果写成这样：

```java
User selectByNameAndAge(String name, int age);
```

👉 **默认会用 `param1`、`param2` 或 `arg0`、`arg1` 识别**

你要么：

- 用 `#{param1}` 和 `#{param2}`
- ✅ 推荐加 `@Param("xxx")` 注解：

```java
User selectByNameAndAge(@Param("name") String name, @Param("age") int age);
```

```xml
SELECT * FROM user WHERE name = #{name} AND age = #{age}
```

---

## ✅ 3. **POJO 类型参数（JavaBean）**

比如你传一个对象进去：

```java
public class User {
    private String name;
    private int age;
    // getter/setter
}
```

方法定义：

```java
User selectByUser(User user);
```

XML 使用：

```xml
SELECT * FROM user WHERE name = #{name} AND age = #{age}
```

✅ MyBatis 会自动从 `user.getName()` 和 `user.getAge()` 中取值。

---

## ✅ 4. **Map 类型参数**

传入 `Map<String, Object>`，灵活但类型不安全。

```java
User selectByMap(Map<String, Object> map);
```

使用方式：

```xml
SELECT * FROM user WHERE name = #{name} AND age = #{age}
```

比如你传的 Map 是：
```java
map.put("name", "Tom");
map.put("age", 25);
```

---

## ✅ 5. **@Param 多参数绑定（推荐）**

可以用于多个参数时指定别名，避免使用 param1/param2。

```java
User select(@Param("name") String name, @Param("age") int age);
```

对应 XML：

```xml
SELECT * FROM user WHERE name = #{name} AND age = #{age}
```

---

## ✅ 6. **集合类型参数（List、Array）**

适用于：`IN (...)` 查询等

### 🔹 List 参数

```java
List<User> selectByIds(@Param("ids") List<Integer> ids);
```

```xml
SELECT * FROM user WHERE id IN
<foreach item="id" collection="ids" open="(" separator="," close=")">
  #{id}
</foreach>
```

### 🔹 Array 参数

```java
User[] selectByArray(@Param("names") String[] names);
```

`<foreach>` 仍然适用。

---

## ✅ 总结表格：

| 类型            | 支持 | 特点说明 |
|-----------------|------|----------|
| 单个简单类型     | ✅   | 参数名自动识别（或用 `#{param1}`） |
| 多个简单类型     | ✅   | 需 `@Param` 或用 `param1`, `param2` |
| JavaBean（POJO）| ✅   | 可直接按属性名访问 |
| Map             | ✅   | 灵活，适合动态拼接 |
| @Param绑定      | ✅   | 多参数推荐方式 |
| List/Array      | ✅   | 结合 `<foreach>` 使用 |

---

有需要我可以直接写一个 `Mapper 接口 + XML + 测试调用` 的例子让你跑一遍。要不要来一个？