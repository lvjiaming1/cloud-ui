### 基本用法

``` html
<u-drawer>
    <template #title>
        <u-text text="标题"></u-text>
    </template>
    <template #body>
        <u-text>内容</u-text>
    </template>
    <template #foot>
        <u-linear-layout vusion-disabled-cut justify="start">
            <u-button color="primary">确定</u-button>
            <u-button>取消</u-button>
        </u-linear-layout>
    </template>
</u-drawer>
```
