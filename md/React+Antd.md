### 1.父、子(Form表单组件)组件都是函数组件，怎么通过Form表单提供的wrappedComponentRef获取表单的值
  step1: 父组件
  ```
  // 引入useRef
  import React, { useState, useEffect, useRef } from 'react';
  // 创建useRef
  const getFormValue = useRef();
  // 绑定wrappedComponentRef属性
  <myForm history={history}  wrappedComponentRef={getFormValue} getList={getList} />
  ```
  step2: 子组件
  ```
  // 引入 useImperativeHandle，forwardRef
  import React, {useImperativeHandle，forwardRef } from 'react';
  // 使用forwardRef转发ref
   Form.create()(forwardRef(MyForm));
   // 组件接收ref
   function MyForm(props,ref){
   // 暴露获取值的方法给父组件
      useImperativeHandle(ref, () => ({
        formFields: props.form.getFieldsValue()
    }))
      return <MyForm >
             </MyForm> 
   }
  ```
