# MDK5-KEIL工程Git提交规范



## 子模块

**固定格式**

```
submodule(<子模块路径>): <操作> <描述> @<版本信息>
```

<br>



**严格规则**

1. **类型固定**：`submodule`
2. **操作仅限 3 种**：`add` / `update` / `remove`
3. **add 必须带**：初始版本（tag /hash）
4. **update 必须带**：目标版本（tag /hash）
5. **版本格式**：`@v1.0.0` 或 `@a1b2c3d`

<br>



**示例**

1. 添加子模块

   ```
   submodule(libs/STM32_HAL): add official HAL library @v1.8.5
   
   submodule(libs/FreeRTOS): add RTOS kernel @v10.4.6
   ```


2. 更新子模块

   ```
   submodule(libs/STM32_HAL): update bugfix version @v1.8.6
   
   submodule(libs/FreeRTOS): update to latest commit @8e2dc4f
   ```

3. 删除子模块

   ```
   submodule(libs/Deprecated): remove unused driver submodule
   ```

   
