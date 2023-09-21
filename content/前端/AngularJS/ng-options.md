# ng-options

## select 标签中 ng-options 的用法

```html
<select ng-options="x.busiSubType as x.busiSubTypeNm for x in busiSubTypeList">
    <option value="">请选择</option>
</select>
```

- 下拉框中显示的值：busiSubTypeNm
- 选中后传入的值：busiSubType