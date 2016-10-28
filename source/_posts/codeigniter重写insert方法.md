---
title: Codeigniter重写insert方法
tags:
  - php
  - Codeigniter
  - mysql
categories:
  - code
date: 2016-10-28 16:35:59
updated: 2016-10-28 16:35:59
permalink:
---

> 使用Codeiginter 框架插入数据时有唯一索引键值存在**解决办法**

　　对数据进行存储的时候，会有一些唯一索引的字段已经有值了，插入数据的时候会失败我们通常解决办法是先查询这个值
是否存在，存在就跟新`update`，不存在就`insert`。以下是重写了Codeigniter 模型中的insert方法,极大的简化了步骤！
<!-- more -->
```php
	/**
	 * 插入单条记录(Insert)
	 *
	 * @param array $params 插入字段及值数组
	 * @param string $table 插入表名，未指定的场景插入主表
	 * @param array $columns 插入表中所属字段名数组，若指定则按此进行过滤
	 * @param array $updateColumns  若唯一键冲突时需要更新的列数组
	 * @return mixed FALSE:没有插入的字段 int:插入记录id
	 *
	 * @throws \C_DB_Exception
	 */
	public function insert($params, $table = NULL, $columns = array(), $updateColumns = array())
	{
		// 如果未指定表名，则认为对本Model层的主表进行操作
		if (empty($table)) {
			$table = $this->table;
			$columns = $this->columns;
		}

		// 过滤掉不属于该表中的字段
		if ( ! empty($columns)) {
			foreach ($params as $column => $val) {
				if (in_array($column, $columns)) {
					$row[$column] = $val;
				}
			}
		}
		// 不过滤
		else {
			$row = $params;
		}

		// 没有插入字段，直接返回
		if ( ! isset($row)) {
			return FALSE;
		}

		// 保证创建时间、更新时间必须设置
		if ( ! isset($row['create_at']) && in_array('create_at', $columns)) {
			$this->load->helper('common_function');
			$datetime = get_datetime();
			$row['create_at'] = $datetime;
			$row['update_at'] = $datetime;
		}

		// 封装SQL文，过滤不安全因素
		$sql = $this->db->insert_string($table, $row);

		// 若唯一键冲突时需要更新的列数组，则再次封装SQL文
		if ( ! empty($updateColumns)) {
			$updateValues = '';
			foreach ($updateColumns as $updateColumn) {
				if (isset($updateValues[0])) {
					$updateValues .= ',';
				}
				$updateValues .= $updateColumn . '=VALUES(' . $updateColumn . ')';
			}
			$sql = $sql . ' ON DUPLICATE KEY UPDATE ' . $updateValues;
		}

		// SQL执行
		$query = $this->db->query($sql);
		// SQL执行失败，抛出异常
		if (FALSE === $query) {
			$code = $this->db->_error_number();
			$msg = $this->db->_error_message();
			C_log::Error($code . ':' . $msg, $this->user_id, $sql, self::STATUS_DB_LOG);
			throw new C_DB_Exception('', C_DB_Exception::DB_ERR_CODE);
		}

		// SQL文写入DB日志
		if (LOG_UPDATE_SQL) {
			C_log::Info('', $this->user_id, $sql, self::STATUS_DB_LOG);
		}

		// 取得插入ID返回
		$id = $this->db->insert_id();
		return $id;
	}

```

> 如果直接写SQL还有一种语句

```SQL
语法： INSERT INTO table  VALUES () ON DUPLICATE KEY UPDATE row='$row'
举个茄子：INSERT INTO it_teacher (uid,name,qq,create_at) VALUES ('$userid','$name','$qq',NOW())  ON DUPLICATE KEY UPDATE name='$name',qq='$qq',phone='$phone'";
```
以上！
