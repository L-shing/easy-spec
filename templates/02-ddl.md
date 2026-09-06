# {{需求名}} DDL（{{需求号}}）

> 对齐 `01-tech-design.md`。正文以可执行 SQL 为主；必要说明用 `--` 注释写在语句上方。  
> 有 ES / 其它存储时，另起代码块即可。  
> **Apollo / Redis / 配置中心写在 `01`，不要写进本文。**

```sql
-- {{简要说明：改了什么 / 注意点}}
ALTER TABLE `{{table}}`
	ADD COLUMN `{{col}}` {{type}} NOT NULL DEFAULT {{val}} COMMENT '{{含义}}';

-- {{建表说明}}
CREATE TABLE `{{table}}` (
	`id` bigint unsigned NOT NULL AUTO_INCREMENT COMMENT '主键',
	-- … 其它列 …
	PRIMARY KEY (`id`),
	KEY `idx_{{name}}` (`{{cols}}`)
) DEFAULT CHARACTER SET=utf8mb4 COMMENT='{{表注释}}';
```
