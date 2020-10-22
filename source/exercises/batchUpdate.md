### 批量更新
#### 第一种
```
UPDATE sys_user t1
SET t1.unitaddr = (select t2.address from sys_unit t2 where t2.id = t1.unitid)
WHERE EXISTS(SELECT 1 FROM sys_unit  where sys_unit.id = t1.unitid);

```

#### 第二种
```
---内联视图更新(需要相关表都设置好了主键---------
UPDATE (
select t1.unitaddr  unitaddr1,t2.address  unitaddr2 from sys_user t1,sys_unit t2 where t2.id = t1.unitid
) t
set unitaddr1 =unitaddr2;
```
#### 第三种
```
merge into sys_user t1
using (select t2.id,t2.address from sys_unit t2) t
on (t.id = t1.unitid)
when matched then 
  update  set t1.unitaddr = t.address;
```