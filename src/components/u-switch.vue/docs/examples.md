### 基本用法

``` html
<u-switch></u-switch>
```

### Value 绑定

``` vue
<template>
<u-switch v-model="value" width="wide" disabled>{{ value ? 'ON' : 'OFF' }}</u-switch>
</template>
<script>
export default {
    data() {
        return { value: true };
    },
};
</script>
```

### 基本状态、只读和禁用

``` html
<u-linear-layout>
    <u-switch with-text></u-switch>
    <u-switch readonly></u-switch>
    <u-switch disabled></u-switch>
</u-linear-layout>
```

### 调整尺寸

``` vue
<template>
<u-switch v-model="value" size="small" style="width: 52px;">{{ value ? 'LIST' : 'LIST' }}</u-switch>
</template>
<script>
export default {
    data() {
        return { value: false };
    },
};
</script>
```
