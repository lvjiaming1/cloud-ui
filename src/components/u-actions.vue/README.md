<!-- 该 README.md 根据 api.yaml 和 docs/*.md 自动生成，为了方便在 GitHub 和 NPM 上查阅。如需修改，请查看源文件 -->

# UActions 操作项

- [示例](#示例)
    - [基本用法](#基本用法)
    - [使用指令](#使用指令)
- [UActions API](#uactions-api)
    - [Props/Attrs](#propsattrs)
    - [Slots](#slots)
- [UAction API](#uaction-api)
    - [Props/Attrs](#propsattrs-2)
    - [Events](#events)

**Table**

当待操作项过多时，会自动收纳

## 示例
### 基本用法

`'u-action'`实际使用组件`'u-link'`进行渲染，所以具备`'u-link'`的所有功能，不需要再其内部使用`'u-link'`组件实现跳转等功能

``` html
<u-linear-layout direction="vertical">
    <u-actions>
        <u-action disabled>设置</u-action>
        <u-action to="/components/u-navbar">跳转到 Navbar</u-action>
        <u-action>重启</u-action>
    </u-actions>
    <u-actions>
        <u-action disabled>设置</u-action>
        <u-action to="/components/u-navbar">跳转到 Navbar</u-action>
        <u-action>重启</u-action>
        <u-action disabled>保存为镜像</u-action>
        <u-action to="/components/u-select">跳转到 Select</u-action>
    </u-actions>
</u-linear-layout>
```

### 使用指令

``` html
<u-linear-layout direction="vertical">
    <u-actions>
        <u-action disabled>设置</u-action>
        <u-action v-tooltip="'跳转到 Navbar'" to="/components/u-navbar">显示Tips</u-action>
        <u-action>重启</u-action>
        <u-action disabled>保存为镜像</u-action>
        <u-action v-tooltip="'跳转到 Select'" to="/components/u-select">显示Tips</u-action>
    </u-actions>
</u-linear-layout>
```

## UActions API
### Props/Attrs

| Prop/Attr | Type | Options | Default | Description |
| --------- | ---- | ------- | ------- | ----------- |
| max-count | number |  | `3` | 一列最多显示链接个数 |
| menu-title | string |  | `'更多'` | 菜单标题 |
| placement | string | `[object Object]`<br/>`[object Object]`<br/>`[object Object]`<br/>`[object Object]`<br/>`[object Object]`<br/>`[object Object]`<br/>`[object Object]`<br/>`[object Object]`<br/>`[object Object]`<br/>`[object Object]`<br/>`[object Object]`<br/>`[object Object]` | `'bottom-end'` | 菜单的弹出方向 |

### Slots

#### (default)

插入`<u-action>`子组件。

## UAction API
### Props/Attrs

| Prop/Attr | Type | Options | Default | Description |
| --------- | ---- | ------- | ------- | ----------- |
| hidden | boolean |  | `false` | 自定义显示/隐藏条件 |
| color | string | `[object Object]`<br/>`[object Object]`<br/>`[object Object]`<br/>`[object Object]`<br/>`[object Object]`<br/>`[object Object]` | `'default'` | 设置颜色 |
| linkType | string | `[object Object]`<br/>`[object Object]`<br/>`[object Object]` | `'href'` | 链接类型 |
| hrefAndTo | string |  |  | 链接地址 |
| target | string | `[object Object]`<br/>`[object Object]`<br/>`[object Object]`<br/>`[object Object]` | `'_self'` | 父级窗口和顶级窗口仅适用于iframe组件嵌套的情况，若不存在嵌套，则打开方式同当前窗口。 |
| decoration | boolean |  | `true` | 是否显示下划线 |
| disabled | boolean |  | `false` | 是否禁用。禁用后不会响应点击事件。 |
| display | string | `[object Object]`<br/>`[object Object]` | `'inline'` | 展示方式 |

### Events

#### @click

点击此项时触发，与原生 click 事件不同的是，它只会在非禁用状态下触发。

| Param | Type | Description |
| ----- | ---- | ----------- |
| $event | MouseEvent | 鼠标事件对象 |

