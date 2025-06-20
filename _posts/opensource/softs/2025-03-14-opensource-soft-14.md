---
layout: post
title: 开源软件推荐系列14:本地数据存取
date: 2025-03-14 05:20:00 +0800
categories: [开源软件推荐系列]
tags: [开源软件推荐系列]
---



---

### 📁 一、轻量级数据存储（推荐指数⭐⭐⭐⭐⭐）
#### 1. **SQLite（嵌入式关系数据库）**
```python
import sqlite3

# 创建/连接数据库（自动生成test.db文件）
conn = sqlite3.connect('local_data.db')
cursor = conn.cursor()

# 建表
cursor.execute('''CREATE TABLE IF NOT EXISTS users 
                  (id INTEGER PRIMARY KEY, name TEXT, age INTEGER)''')

# 写入数据
cursor.execute("INSERT INTO users (name, age) VALUES (?, ?)", ('Alice', 28))
conn.commit()  # 提交事务

# 读取数据
cursor.execute("SELECT * FROM users")
print(cursor.fetchall())  # 输出: [(1, 'Alice', 28)]

conn.close()  # 关闭连接
```
- **优势**：无需安装服务端，单文件存储，支持ACID事务
- **适用**：用户配置、交易记录等结构化数据

#### 2. **JSON文件（配置/简单数据）**
```python
import json

# 写入数据
data = {"user": "Alice", "prefs": {"theme": "dark", "notify": True}}
with open('config.json', 'w') as f:
    json.dump(data, f)  # 可选indent=4美化格式

# 读取数据
with open('config.json') as f:
    loaded = json.load(f)
    print(loaded['prefs']['theme'])  # 输出: dark
```
- **优势**：人类可读，跨语言兼容
- **注意**：频繁写入大文件时性能较低

---

### ⚡ 二、高性能场景方案（推荐指数⭐⭐⭐⭐）
#### 3. **Shelve（类字典持久化）**
```python
import shelve

# 写入数据（自动创建data.db文件）
with shelve.open('data') as db:
    db['user1'] = {'name': 'Bob', 'score': 85}
    db['config'] = {'version': 1.2}

# 读取数据
with shelve.open('data') as db:
    print(db['user1'])  # 输出: {'name': 'Bob', 'score': 85}
```
- **本质**：基于pickle的键值存储
- **适用**：需要快速键值查询的非结构化数据

#### 4. **HDF5（科学计算大文件）**
```python
import h5py
import numpy as np

# 写入大型数组
data = np.random.rand(10000, 10000)
with h5py.File('big_data.h5', 'w') as f:
    f.create_dataset('matrix', data=data)

# 读取数据
with h5py.File('big_data.h5', 'r') as f:
    loaded = f['matrix'][:]  # 加载整个数组
```
- **需安装**：`pip install h5py`
- **适用**：数值计算、机器学习数据集

---

### 🔒 三、安全敏感数据方案（推荐指数⭐⭐⭐）
#### 5. **SQLite + 加密扩展（SQLCipher）**
```python
# 安装: pip install pysqlcipher3
from pysqlcipher3 import dbapi2 as sqlite

conn = sqlite.connect('encrypted.db')
cursor = conn.cursor()
cursor.execute("PRAGMA key='MySecretKey'")  # 设置加密密钥

# 后续操作与普通SQLite相同
cursor.execute("CREATE TABLE secrets (id INT, info TEXT)")
```
- **安全性**：AES-256加密整个数据库文件
- **代价**：读写性能下降约10-15%

---

### 📊 方案对比速查表
| **存储需求**          | 推荐方案        | 性能 | 易用性 | 安全 |
|-----------------------|---------------|------|--------|------|
| 结构化数据（中小规模） | SQLite        | ⭐⭐⭐⭐ | ⭐⭐⭐⭐  | ⭐⭐   |
| 简单配置/元数据       | JSON文件      | ⭐⭐   | ⭐⭐⭐⭐⭐ | ⭐    |
| 键值型数据            | Shelve        | ⭐⭐⭐  | ⭐⭐⭐⭐  | ⭐    |
| 大型数值数据          | HDF5          | ⭐⭐⭐⭐⭐| ⭐⭐⭐   | ⭐    |
| 敏感结构化数据        | SQLite+加密   | ⭐⭐⭐  | ⭐⭐⭐   | ⭐⭐⭐⭐ |

---

### ⚠️ 关键实践建议
1. **备份机制**：定期压缩备份数据文件
   ```python
   import shutil
   shutil.make_archive('backup_2023', 'zip', 'data_dir')  # 目录压缩
   ```
2. **异常处理**：增加写入保护
   ```python
   try:
       with open('data.json', 'w') as f:
           json.dump(...)
   except IOError as e:
       print(f"写入失败! 错误: {str(e)}")
   ```
3. **性能优化**：
   - SQLite批量写入使用`executemany()`
   - 大文件避免一次性加载：HDF5支持分块读取
4. **兼容性**：路径处理用`pathlib`
   ```python
   from pathlib import Path
   data_path = Path('data') / 'v1' / 'records.db'  # 自动处理操作系统路径差异
   ```

> 根据数据规模选择方案：  
> - **<1MB**：JSON/Shelve  
> - **1MB~1GB**：SQLite  
> - **>1GB**：HDF5  
> - **敏感数据**：必须加密存储

