---
 title: <% tp.file.title %>
 author: feng.wang
 create_time: <% tp.file.creation_date() %>
 description: 
---

<% await tp.file.move("/Output/study/" + ((tp.file.title.includes("未命名") || tp.file.title.toLowerCase().includes("untitled")) ? (await tp.system.prompt("请输入要创建的文件名")) : tp.file.title)) %>
