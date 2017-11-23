# REST API风格指南
- URL中集合资源的操作使用名词复数。

	Don't
	
	```
	// 查询所有用户
	GET /examples/user 
	GET /examples/user/list
	
	// 查询所有用户的所有图书
	GET /examples/user/book
	GET /examples/user/book/list
	```
	
	Do
	
	```
	// 查询所有用户
	GET/examples/users
	
	// 查询所有用户的所有图书
	GET /examples/users/books 
	```

- URL中对单个资源的操作可以使用单数。

	Don't
	
	```
	// 查询id为101的用户的学校信息
	GET /examples/users/101/schools
	```
	
	Do
	
	```
	// 查询id为101的用户的学校信息(假定某一时刻所在的学校只有一个)
	GET /examples/users/101/school
	```

- URL中资源的命名需要由多个单词构成时，使用中划线(`-`)连接。

	Don't
	
	```
	// 查询id为101的用户的用户名
	GET /examples/users/101/user_name
	GET /examples/users/101/userName
	```
	
	Do
	
	```
	// 查询id为101的用户的用户名
	GET /examples/users/101/user-name
	```


- URL中不要出现动词，使用`HTTP Methods`表示对URL资源的操作动作。

	Don't
	
	```
	// 更新用户信息
	POST /examples/users/update 
	POST /examples/updateUser
	```
	
	Do
	
	```
	// 更新用户信息
	PUT /examples/users
	```
- 对于资源集合中的某一具体资源的操作，建议将具体资源的标识符放入URL中。
	Don't
	
	```
	// 查询id为101的用户信息
	GET /examples/users?id=101
	
	// 查询id为101的用户的所有图书
	GET /examples/users/books?id=101
	```

	Do
	
	```
	// 查询id为101的用户信息
	GET /examples/users/101
	
	// 查询id为101的用户的所有图书
	GET /examples/users/101/books
	```
	
- URL中的每一级遵循资源范围上`从大到小`、`从广到窄`的排列原则，URL的末尾是当前操作的目标。
	
	Don't
	
	```
	// 查询id为101的用户的所有图书的所有作者
	GET /examples/books/authors/users/101 // 这个查询操作的目标应该是图书作者，而非用户。
	```
	
	Do
	
	```	
	// 查询id为101的用户的所有图书的所有作者
	// 资源范围从广到窄应该是"所有用户的集合"-->"某个用户"-->"某个用户拥有的所有图书"-->"某个用户拥有图书的作者集合"，而当前操作的目标应该是"作者"这一资源
	GET /examples/users/101/books/authors
	```

- `Request`和`Response`字段一律小写(若由多个单词组成，则用下划线(`_`)连接多个单词)。

	Don't
	
	```
	GET /examples/users?UserName=voidint 
	```

	Do
	
	```
	GET /examples/users?user_name=voidint
	```
	
- 操作失败情况下，不能使用`200`作为`HTTP Status code`，使用合适的[HTTP Status Codes](https://httpstatuses.com/)。

	Don't
	
	```
	// 查询系统中并不存在的id为101的用户
	GET /examples/users/101 
	Response HTTP Status Code: 200
	Response Body: {"status": "success", "message": "user(id=101) not found"}
	```

	Do
	
	```
	// 查询系统中并不存在的id为101的用户
	GET /examples/users/101 
	Response HTTP Status Code:404
	Response Body: {"status": "success", "message": "user(id=101) not found"}
	```