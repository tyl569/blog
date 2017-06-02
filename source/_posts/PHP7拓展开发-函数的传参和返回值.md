---
title: PHP7拓展开发--函数的传参和返回值
date: 2017-05-31 20:26:22
tags: PHP7
---

#### 前言
这次，我们将演示如果在PHP拓展中接收传参和输出返回值
实现大代码效果和下面一样
```php
<?php
function default_value($type, $value = null) {
	if ($type = 'int') {
		return $value ?? 0;
	} else if ($type = 'bool') {
		return $value ?? false;	
	} else if ($type = 'str') {
		return is_null($value) ? "" : $value;
	}
	return null;
	
}

    var_dump(default_value("int"));
    var_dump(default_value("int", 1));
    var_dump(default_value("bool"));
    var_dump(default_value("bool", true));
    var_dump(default_value("str"));
    var_dump(default_value("str", "a"));
    var_dump(default_value("array"));


?>
```

#### 代码实现
##### 基础代码
```cpp
PHP_FUNCTION(default_value)
{
    zend_string     *type;    
    zval            *value = NULL;

#ifndef FAST_ZPP
    /* Get function parameters and do error-checking. */
    if (zend_parse_parameters(ZEND_NUM_ARGS(), "S|z", &type, &value) == FAILURE) {
        return;
    }    
#else
    ZEND_PARSE_PARAMETERS_START(1, 2)
        Z_PARAM_STR(type)
        Z_PARAM_OPTIONAL
        Z_PARAM_ZVAL_EX(value, 0, 1)
    ZEND_PARSE_PARAMETERS_END();
#endif

    if (ZSTR_LEN(type) == 3 && strncmp(ZSTR_VAL(type), "int", 3) == 0 && value == NULL) {
        RETURN_LONG(0);
    } else if (ZSTR_LEN(type) == 3 && strncmp(ZSTR_VAL(type), "int", 3) == 0 && value != NULL) {
        RETURN_ZVAL(value, 0, 1); 
    } else if (ZSTR_LEN(type) == 4 && strncmp(ZSTR_VAL(type), "bool", 4) == 0 && value == NULL) {
        RETURN_FALSE;
    } else if (ZSTR_LEN(type) == 4 && strncmp(ZSTR_VAL(type), "bool", 4) == 0 && value != NULL) {
        RETURN_ZVAL(value, 0, 1); 
    } else if (ZSTR_LEN(type) == 3 && strncmp(ZSTR_VAL(type), "str", 3) == 0 && value == NULL) {
        RETURN_EMPTY_STRING();
    } else if (ZSTR_LEN(type) == 3 && strncmp(ZSTR_VAL(type), "str", 3) == 0 && value != NULL) {
        RETURN_ZVAL(value, 0, 1); 
    } 
    RETURN_NULL();
}
```