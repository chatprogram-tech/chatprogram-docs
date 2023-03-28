# ChatProgram API 文档

## REST API
### 鉴权
`Authorization: Bearer XXX`

### 根据单个文档生成代码
```shell
curl --location 'https://chatprogram.tech/api/code-simple' \
--header 'Authorization: Bearer XXX' \
--header 'Content-Type: application/json' \
--data '{
    "lang": "rust",
    "purpose": "calculate sum of two numbers from request"
}'

```
```json
{
    "full_response": "Certainly! Here is an example program in Rust that will read two numbers from the standard input, add them together, and then print the result:\n\n```rust\nuse std::io;\n\nfn main() {\n    println!(\"Enter the first number:\");\n    let mut first_num = String::new();\n    io::stdin().read_line(&mut first_num).expect(\"Failed to read line\");\n    let first_num: i32 = first_num.trim().parse().expect(\"Please type a number!\");\n\n    println!(\"Enter the second number:\");\n    let mut second_num = String::new();\n    io::stdin().read_line(&mut second_num).expect(\"Failed to read line\");\n    let second_num: i32 = second_num.trim().parse().expect(\"Please type a number!\");\n\n    let sum = first_num + second_num;\n\n    println!(\"The sum of {} and {} is: {}\", first_num, second_num, sum);\n}\n```\n\nYou can run this program by creating a new file called `main.rs` and saving the code to it. Then, compile the program using the Rust compiler by running `rustc main.rs`. Finally, run the program by typing `./main` in the terminal.",
    "sources": [
        {
            "path": null,
            "doc_path": null,
            "lang": null,
            "code": "use std::io;\n\nfn main() {\n    println!(\"Enter the first number:\");\n    let mut first_num = String::new();\n    io::stdin().read_line(&mut first_num).expect(\"Failed to read line\");\n    let first_num: i32 = first_num.trim().parse().expect(\"Please type a number!\");\n\n    println!(\"Enter the second number:\");\n    let mut second_num = String::new();\n    io::stdin().read_line(&mut second_num).expect(\"Failed to read line\");\n    let second_num: i32 = second_num.trim().parse().expect(\"Please type a number!\");\n\n    let sum = first_num + second_num;\n\n    println!(\"The sum of {} and {} is: {}\", first_num, second_num, sum);\n}"
        }
    ],
    "errors": []
}
```

### 根据多个文档生成代码
```shell
curl --location 'https://chatprogram.tech/api/code-workspace' \
--header 'Authorization: Bearer XXXXX' \
--header 'Content-Type: application/json' \
--data '{
    "name": "test",
    "lang": "python",
    "docs": [
        {
            "path": "test.md",
            "code": "please write a python program that constantly output hello world"
        },
        {
            "path": "test2.md",
            "code": "please write a rust program that constantly output hello world"
        }
    ],
    "codes": []
}'
```

```json
{
    "full_response": "For doc test.md\nSure, here's a Python program that constantly outputs \"Hello, World!\" to the terminal:\n\n```python\nwhile True:\n    print(\"Hello, World!\")\n```\n\nNote that this program will run indefinitely, so you'll need to manually interrupt it (e.g. by pressing Ctrl+C on the keyboard) to stop it.\nFor doc test2.md\nCertainly! Here is the Rust code that will output \"Hello, world!\" indefinitely:\n\n```rust\nfn main() {\n    loop {\n        println!(\"Hello, world!\");\n    }\n}\n```\n\nThis program uses a loop to constantly print the message \"Hello, world!\" to the console.\n",
    "sources": [
        {
            "path": "test.1.txt",
            "doc_path": "test.md",
            "lang": null,
            "code": "while True:\n    print(\"Hello, World!\")"
        },
        {
            "path": "test2.1.txt",
            "doc_path": "test2.md",
            "lang": null,
            "code": "fn main() {\n    loop {\n        println!(\"Hello, world!\");\n    }\n}"
        }
    ],
    "errors": []
}
```

## WebSocket API
### 鉴权
`wss://chatprogram.tech/ws-stream/?token=XXX`
或者
`Authorization: Bearer XXX`

### 根据单个文件生成代码
请求
```json
{
    "Code": {
        "lang": "python",
        "purpose": "please write a program that outputs hello world"
    }
}
```
Segment响应: 中间结果
```json
{
    "Segment": {
        "doc": null,
        "delta": "Certainly! Here's a Python program that prints \"Hello, world!\" to the console:\n\n```"
    }
}
```
Complete响应：最终结果
```json
{
    "Complete": {
        "errors": [],
        "full_response": "Certainly! This is a simple program that outputs \"Hello, world!\" to the console:\n\n```python\nprint(\"Hello, world!\")\n``` \n\nJust copy this code and run it in Python environment to see the output.",
        "sources": [
            {
                "code": "print(\"Hello, world!\")",
                "doc_path": null,
                "lang": null,
                "path": null
            }
        ]
    }
}
```
### 根据多个文件生成代码
请求
```json
{
    "Workspace": {
        "name": "test",
        "lang": "laf",
        "docs": [
            {
                "path": "test1.md",
                "code": "please write a program that outputs hello world"
            },
            {
                "path": "test2.md",
                "code": "please write a program that outputs the user's ip address"
            }
        ]
    }
}
```
Segment响应: 中间结果
```json
{
    "Segment": {
        "doc": "test1.md",
        "delta": "Certainly! Here's a Python program that prints \"Hello, world!\" to the console:\n\n```"
    }
}
```
Complete响应：最终结果
```json
{
    "Complete": {
        "errors": [],
        "full_response": "For doc test1.md\nCertainly! Here's a Python program that prints \"Hello, world!\" to the console:\n\n```python\nprint(\"Hello, world!\")\n```\n\nSave this code in a file with a `.py` extension, then run the file in the terminal by typing `python filename.py`, replacing \"filename\" with the name of your Python file.\nFor doc test2.md\nCertainly! Here is a Python program that outputs the user's IP address:\n\n```python\nimport socket\n\nhostname = socket.gethostname()\nip_address = socket.gethostbyname(hostname)\n\nprint(\"Your IP Address is: \" + ip_address)\n```\n\nThis program uses the `socket` module to get the user's hostname and then looks up the IP address associated with that hostname. It then prints out the IP address to the console.\n",
        "sources": [
            {
                "code": "print(\"Hello, world!\")",
                "doc_path": "test1.md",
                "lang": null,
                "path": "test1.1.txt"
            },
            {
                "code": "import socket\n\nhostname = socket.gethostname()\nip_address = socket.gethostbyname(hostname)\n\nprint(\"Your IP Address is: \" + ip_address)",
                "doc_path": "test2.md",
                "lang": null,
                "path": "test2.1.txt"
            }
        ]
    }
}
```