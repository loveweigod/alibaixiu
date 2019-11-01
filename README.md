# 阿里百秀笔记

## I、注意事项

1. 403 -- 一般是参数错了，这时候看 参数、单词
2. 404 -- 说明是接口地址或者参数错了，优先检查 接口地址，然后是参数
3. 304 -- 忽略，说明是从本地缓存中读取文件
4. 5xx -- 服务器错了……

其他：
1. 代码在上线阶段，把项目中的 console.log 以及 alert 全部去掉

## II、开发模式

1. 混合开发模式

- 就是指前后端没有分离的项目
- 前端只需要写 HTML 和 css 就可以，写好之后给后台
- 后台接收到前端页面之后，开始使用后端模板引擎嵌套页面
- 缺点：代码混合到一块，不易于后期的维护


2. 前后端分离开发模式

- 前端需要写 HTML 和 css 以及 ajax 和 js
- 后台操作数据库，给前端返回 json 格式的数据
- 前端使用 ajax 请求后台提供的接口，后台接口返回 json 格式的数据
  前端开始使用前端模板引擎嵌套页面
- 优点：前后端实现了分离，职责明确，方便后期的维护


## III、阿里百秀项目架构

1. 数据库：MongoDB
2. 逻辑层：Node.js、Express 以及 中间件
3. 前  端：jQuery、BootStrap、art-template

### -----用户管理（CRUD）-----

#### 一、登录功能

1. 给登录按钮绑定点击事件
2. 获取邮箱和密码
3. 判断邮箱和密码是否为空，如果是空，需要给提示，同时阻止代码往下运行
4. 发送 ajax 请求
5. 后台肯定会返回请求结果，成功的，进入网站的后台，如果失败，就给用户进行提示

#### 二、退出功能

1. 给退出绑定点击事件
2. 询问用户是否确定退出
3. 发送 ajax 请求


#### 三、用户添加

1. 给添加按钮绑定点击事件
2. 给 input 添加 name 属性，**注意：** 属性值需要和接口给的参数一致
3. $(this).serialize() 获取 form 表单中所有的 name 值
4. 发送 ajax 请求
5. 根据返回结果进行下一步的处理 


#### 四、用户头像上传

1. 给用户头像上传控件，input标签绑定 change 事件
2. 使用 new FormData() 获取文件的信息，使用下标获取具体的图片信息  files[0]
3. 开发发送 ajax 就可以
4. 在请求成功后的回调函数中，获取到头像的地址，同时赋值给页面上的 image
5. 创建隐藏域，把回调函数中返回的地址赋值给隐藏域


#### 五、用户列表展示功能

1. 打开页面，发送ajax请求，获取后台返回的json格式的数据
2. 引用模板文件，定义模板
3. 使用template()方法,将模板和数据进行拼接
4. 获取父级容器，把生成的节点添加到父级容器当中

#### 六、用户信息编辑(修改)

1. 利用事件委托的方法为编辑按钮添加绑定事件，并获取被点击用户唯一的id值
2. 发送ajax请求，根据id值获取当前用户的信息
3. 定义模板(使用template()方法,将模板和数据进行拼接;获取父级容器，把生成的节点添加到父级容器当中)，将获取到的用户信息展示到页面左侧
4. 将更新后的数据同步到数据库中---利用事件委托的方法为表单注册submit提交事件(使用serialize()方法获取用户在表单中输入的所有信息)，并阻止表单的默认提交行为
5. 更新数据，发送ajax请求,点击保存按钮，重新刷新渲染页面


#### 七、用户删除操作

1. 利用事件委托为删除按钮添加点击事件，并获取当前用户唯一的id值
2. 和管理员确认是否真的删除当前用户
3. 发送ajax请求，根据用户id删除当前用户

#### 八、批量删除用户操作

1. 正选---点击全选input框让其处于勾选或者未勾选状态，并让每个用户前面的input框也同步跟随相应发生改变(处于勾选或者未勾选状态),如果处于勾选状态让批量删除按钮显示，否则不显示
2. 反选---利用事件委托的方法为每个用户前面的input框注册change事件,点击每个用户前面的input框，根据用户数量和选中的input框的数量判断是否为全部选中状态，如果为全部选中状态则让全选的input框也处于选中状态(如果选中的按钮数量大于0则显示批量删除按钮，否则不显示)，否则不让其处于选中状态
3. 根据每个用户前面的input的状态，获取元素，然后询问管理员是否真的删除当前选中用户,创建一个空的数组，使用each方法,循环获取勾选状态用户的id值,并将获取的id值存放到数组中
4. 发送ajax请求，并使用join()方法处理数组中的数据,点击确认按钮，重新渲染页面

#### 九、用户密码修改(存在bug待修复)

1. 为修改密码表单的每一个input框添加name属性，name中的属性值要和接口中的参数名一致
2. 为修改密码表单注册submit提交事件，在处理函数中，阻止表单默认的提交行为
3. 使用serialize()方法获取用户在表单输入的所有信息
4. 发送ajax请求，实现密码修改功能，如果密码修改成功，跳转到登录页面，让用户重新登录

### -----分类管理(CRUD)-----

#### 一、添加分类功能
1. 为添加分类表单中的每一个input框添加name属性,name中的属性值要和接口中的参数一致
2. 为添加分类表单注册submit表单提交事件，在处理函数中，阻止表单默认的提交行为
3. 使用serialize()方法获取用户在表单中输入的所有信息
4. 发送ajax请求，实现分类添加功能

#### 二、分类列表数据展示功能
1. 打开页面，发送ajax请求，获取分类页面的数据
2. 引用模板文件，定义模板
3. 使用template()方法将模板和数据进行拼接
4. 获取父级容器，将生成的节点动态的添加到父级容器当

#### 三、分类列表编辑（修改）操作
1. 利用事件委托为编辑注册点击事件，并在处理函数中获取到当前所选中的分类信息的唯一id值(利用自定义属性，把对应的分类的id设置    到自定义属性中)
2. 发送ajax请求，根据获取到的id值获取到当前选中分类的详细信息
3. 定义模板(使用template()方法，将模板和数据进行拼接，获取父级容器，将生成的节点动态的添加到父级容器当中)，将获取到的分类信息展示到页面中
4. 将更新后的数据同步到数据库中---利用事件委托的方法为表单注册submit提交事件(使用serialize()方法获取用户在表单中输入的所有信息)，并阻止表单的默认提交行为
5. 更新数据后，发送ajax请求，点击更新保存按钮，重新刷新渲染页面

#### 四、分类列表删除操作
1. 利用事件委托的方法为删除按钮注册点击事件，并在处理函数中获取到当前选中信息的唯一id值(利用自定义属性，把对应分类的id值设    置到自定义属性中)
2. 和管理员确认书否真的删除当前选中的分类信息
3. 发送ajax请求，根据获取到的当前分类id进行删除操作,若果删除成功，重新刷新渲染页面

### -----文章管理(CRUD)-----
