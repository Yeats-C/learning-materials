# postman测试传入List<String>参数

## 第一步：设置headers
  ![image](https://user-images.githubusercontent.com/64882640/123889696-9dbb2780-d988-11eb-92bc-0e7fb662e79c.png)
* Content-type 的值为application/json

 ## 第二步：传值参数list<String>
* 在body中，传值参数，list<String> 使用[]括起来
  ![image](https://user-images.githubusercontent.com/64882640/123889752-b88d9c00-d988-11eb-96ce-3f67d1cbb134.png)
  
## 第三步：controller层设置两个注解@ResponseBody 和@RequestBody
