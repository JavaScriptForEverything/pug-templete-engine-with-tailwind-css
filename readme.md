## Tailwind-css + Pug Templete

I can seperate pug into 3 section
* HTML section
* Javascript section
* Templete / Layout


#### Key topics
* Indentation                           # Like python code, Indentation desice how code react
* HTML Section: 
        * Single line vs Multi-line Text
        * Nestes Tags in 2 ways
        * Attributes
        * Styles                        # Embeded + lnline + External CSS

* JS Section: 
        * Variables                     # Server-Side Variables
        * Variables in html attribute   # Server-Side Variable in html Attribute
        * Array, Object multi-line      # Server-Side Variables
        * Condition                     # Server-Side Condition
        * Loop                          # Server-Side Loop
        * Mixin / Function              # Server-Side Function in pug file

        * Client Side JS                # Embeded + External JS
        * Sending data from Server-Side to Client-Side





# 
#### Creating Project

```
$ yarn init -y 		                # Make sure `node` and `yarn` installed

$ mkdir pug 			        # input pug files will be placed here
$ mkdir html 			        # Generated output html files will be here
```


#### Configure Pug Templete Engine to generate HTML

```
$ yarn add -D pug-cli
$ yarn pug -h

$ touch pug/index.pug 	                # this file will be generate /html/index.html file
$ yarn pug ./pug --out html --pretty    # 

```

###### /pug/index.pug
- We can generate boiler plate code by (`!`) 	: Emmet works in .pug exactly as does in .html file

```
doctype html
html(lang="en")
	head
		meta(charset="UTF-8")
		meta(name="viewport", content="width=device-width, initial-scale=1.0")
		title Testing 

	body 
		hi HTML

		button(type="button") Click
```

###### Generated file => `/html/index.html` 
```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Testing </title>
    <link rel="stylesheet" href="style.css">
  </head>
  <body> 
    <hi>HTML</hi>
    <h2>Hi there</h2>
    <button type="button">Click</button>
  </body>
</html>
```


#### Using `Tailwind-CSS` in pug

```
$ yarn add -D tailwindcss
$ yarn tailwindcss -h
$ yarn tailwindcss init                 # Will generate `tailwind.config.js`

$ touch style.css                       # See style.css file bellow
$ yarn tailwindcss --input style.css --output html/style.css --watch
```

###### /tailwind.config.js
- Just add .pug file to watch

```
...
  content: ['**/*.pug'],
...
```

###### /style.css
```
@tailwind base;
@tailwind components;
@tailwind utilities;
```




#### Creating Scripts for tailwind and pug
We have to run 2 script in 2 terminal simentinously, one for tailwind and one for pug
if we need live-server then we have to run web-server into another terminal.

To solve this problems we will use `concurrently` npm package which run multiple process by single script

```
        $ yarn add -D concurrently
        $ yarn concurrently " \"echo first script/" /"echo second script/" "
```


###### /package.json
```
...
"scripts": {
        "tailwind-dev"  : "tailwindcss --input ./style.css --output ./html/style.css --watch",
        "pug-dev"       : "pug pug --out ./html --pretty --watch",
	"dev"           : "concurrently --kill-others \"yarn tailwind-dev\" \"yarn pug-dev\" "
},
...

$ yarn dev              : Run the command
```

##### WebServer
If we need another `live-server` we can add another script and add that after end of 
the `concurrently` script but VSCode has live-server extention which run seperate terminal 
which is run as `JOB` (process runs in background).



# 


##### HTML Section: Indentation
```
doctype html                            # 
html(lang="en")                         # Root Level
	head                            # inside Root
		title Testing           # inside head, which is inside Root     : 3 level indentation
	body                            # 
		h1 Hi there             # 
```


##### HTML Section: Single line vs Multi-line Text
We can write multi-line text in 2 ways:
01. By Pipe Symble in next line (must be properly indented)
02. By dot immediately after html tag 

```
doctype html
html(lang="en")
	head
		title Testing 
	body 
		p This is single line pragraph

                h1 Method-1:
		p 
			| This is multi-line pragraph: line One
			| This is multi-line pragraph: line Two

                h1 Method-2:
		p.
		        This is multi-line pragraph: line One
			This is multi-line pragraph: line Two

```



##### HTML Section: Nestes Tags in 2 ways
HTML Nexted tags can be used in 2 ways:
01. By indentation one after another
02. By clone (`:`) immediately after previous tag


```
html
        head
                title
        body
		h1 Nested Tags

		h3 Method-1:
		ul
			li
				a(href="")  Link 1
			li
				a(href="") Link 2

		h3 Method-2:
		ul
			li: a(href="") Link 1
			li: a(href="") Link 2
```


##### HTML Section: HTML Attributes
We can pass any html attribute inside first bracket immediately after the gat
it can be in single line or multi-line

id and class has special syntas along side regular syntas 
- #id-name
- .class-name

`input(type="text")`

`input(type="text" placehoder='placeholder' name='username' id='username' value='') `

```
input.border.border-blue-500(
        type="text" 
        placehoder='placeholder' 
        name='username' 
        id='username' 
        value=''
)
```


Example:
```
doctype html
html(lang="en")
	head
		title Testing 
		link(rel="stylesheet", href="style.css")

	body 
		p#userId Paragraph with id attribute
		p.userId Paragraph with class attribute

		p(class='text-red-500') Paragraph with class attribute

		div(class='border border-red-200 my-4')
			label(for="username") Username
			input.border.border-blue-500(type="text" placehoder='placeholder' name='username' id='username' value='')
```

##### HTML Section: Using Styles 
```
doctype html
html(lang="en")
	head
		title Testing 
		link(rel="stylesheet", href="style.css")
		style.
			.my-class {
				border: 1px solid red;
				color: red;
			}

	body 
		p(style='border: 1px solid red; color: red;') Inline Styles
		p(class='my-class') Embeded Styles
		p(class='border border-red-500 text-red-500') Tailwind CSS

```


#### Javascript Section
We can use javascript in pug file too, but when we use javascript we have 2 type of javascript
01. Server-Side: When we use any js code inside pug file except inside script tag.
02. Client-Side: Any JS code used by script tag
03. Server-Side => Client-Side: We can send variable from server-side to client-side too


##### Javascript Section: (Server-Side) Variables
Server-Side JS run in ServerSide by node, and logs shows inside terminal not inside browser console.

Server-Side: variables can execute in 2 ways
- #{ variable_name }            : Pug Syntax for variable
- tag=variable_name             : `=` immediately after html tag (no space)

Server-Side: Single line vs Multi-line
- Single-line: prefix (`-`) before every line 
- Multi-line:  prefix (`-`) then next line indented 



```
doctype html
html(lang="en")
	head
		title Testing 

	body 
		h1 Server-Side JS (Show logs in Terminal not browser-console)
		
		- console.log('Hi Server-Side')
		- const username = 'Riajul Islam'
		- 
			const users = [
				{ name: 'Riajul Islam', age: 28 },
				{ name: 'Fiaz Sofone', age: 24 },
				{ name: 'Arian hossain ayan', age: 8 },
			]

		p My name is #{username}
		p= `My name is ${username}`

		ul
			- users.forEach(({ name, age }) => {
				li= name + ' ' + age
			- })
```

##### Javascript Section: (Server-Side) Loop
```
doctype html
html(lang="en")
	head
		title Loop 

	body 
		h1 Server-Side Loop
		hr
		
		- 
			const users = [
				{ name: 'Riajul Islam', age: 28 },
				{ name: 'Fiaz Sofone', age: 24 },
				{ name: 'Arian hossain ayan', age: 8 },
			]

		h1 Javascript loop
		ul
			- users.forEach(({ name, age }, index) => {
				li= index + ': ' + name + ' ' + age
			- })


		h1 Pug built-in loop (can't use Object Destructring Syntax)
		ul
			each user,index in users
				li #{index}: #{user.name} #{user.age}

```

##### Javascript Section: (Server-Side) Condition
```
doctype html
html(lang="en")
	head
		title Condition 

	body 
		h1 Server-Side Condition
		hr
		
		- 
			const users = [
				{ name: 'Riajul Islam', age: 28 },
				{ name: 'Fiaz Sofone', age: 24 },
				{ name: 'Arian hossain ayan', age: 8 },
			]

		h1 Javascript Condition
		ul
			- users && users.forEach(({ name, age }, index) => {
				li= index + ': ' + name + ' ' + age
			- })


		h1 Pug built-in Condition 

		ul
			if users
				each user,index in users
					li #{index}: #{user.name} #{user.age}
			else 
				li No users found


		- const isExists = false
		ul
			if !isExists
				each user,index in users
					li #{index}: #{user.name} #{user.age}
			else 
				li No users found


		ul

			if isExists
				p Thay exists

			else if users.length
				each user,index in users
					li #{index}: #{user.name} #{user.age}

			else 
				li No users found

```

##### Javascript Section: (Server-Side) mixin (Function)
In javascript we can use function, same thing can be achive via `mixin` in pug

- again make sure maintain the indentation
- mixin invoke with (`+`) prefix


```
doctype html
html(lang="en")
	head
		title Function || Mixin 

	body 
		h1 Server-Side mixin
		hr
		
		- 
			const users = [
				{ name: 'Riajul Islam', age: 28 },
				{ name: 'Fiaz Sofone', age: 24 },
				{ name: 'Arian hossain ayan', age: 8 },
			]

		h1 Javascript Function/Mixin

		mixin userList (props = {})
			- console.log('user list')
			- 
				const {
					name='Your Name',
					age 
				} = props
			
			div(style='display: flex; gap: 20px; align-items: center;')
				p= name
				span= age
		

		each user in users
			+userList()
			//- +userList({ name: user.name, age: user.age })
			//- +userList(user)
```


##### Javascript Section: (Client-Side) 
Client-Side JS run in ClientSide by Browser, and logs shows browser console.

We can use client-side js in 2 ways:
01. External js file    : script(src='./client.js') 
02. Embeded js          : script.

client.js              : /html/client.js : where .html files,  not .pug files
index.pug              : /pug/index.pug


```
doctype html
html(lang="en")
	head
		title Client-Side JS
		script(src="client.js") 

	body 
		h1 Client-Side JS
		hr
		
		h1 Server-Side: Javascript 
		- 
			const users = [
				{ name: 'Riajul Islam', age: 28 },
				{ name: 'Fiaz Sofone', age: 24 },
				{ name: 'Arian hossain ayan', age: 8 },
			]

		- console.log('Server-Side: log one')
		- console.log(users)


		h1 Client-Side: Javascript 

		script.
			console.log('Client-Side: log one')
			console.log('Client-Side: log two')
```



##### Javascript Section: (Server-Side => Client-Side) 
Sending Variables from server-side to client-side

To send data from Server-Side to Client-Side, must follow 2 conditions:

1. Data must by string, because object, function, ... can't pass throught network or wires.
2. To capture server-side variable inside client-side use special syntax: `!{ serverSideVariable }


```
doctype html
html(lang="en")
	head
		title Server-Side to Client-Side JS

	body 
		h4 Sending data from Server-Side to Client-Side:
		hr
		
		h4 Server-Side: Javascript 
		- 
			const users = [
				{ name: 'Riajul Islam', age: 28 },
				{ name: 'Fiaz Sofone', age: 24 },
				{ name: 'Arian hossain ayan', age: 8 },
			]

		- const usersJs = JSON.stringify(users)                 (1) : Convert to String to send to client


		h4 Client-Side: Javascript 

		script.
			const usersJs = !{usersJs}                      (2) : Capture via special syntax `!{}`
			console.log(usersJs)

			const users = !{JSON.stringify(users)}          (1,2) : do both in single line
			console.log(users)
```



#### Templete or Layout
Pug has special syntax to create `templete`  or page `layout` without duplicating codes.
via some special keyworks:

* `include <filename>`                          : Include any .pug file 
* `extends <filename>`                          : Same as include but can override any section or `block`
        * `block <word>`
        * `block append <word>`
        * `block prepend <word>`
