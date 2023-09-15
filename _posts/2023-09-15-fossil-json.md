---
layout: post
title: Using Fossil as a JSON database
---
This is an example of how to use [Fossil](https://fossil-scm.org/home/doc/trunk/www/index.wiki) as a JSON database. What we'll do is use the tickets database to store records in JSON format, and then we'll query those records using SQLite. The tickets database will be pushed to the remote repository and be fully version controlled automatically.

```
fossil ticket -R learn.fossil add comment '{"status":"open","content":"This is a test of Fossil as a distributed database"}'
fossil ticket -R learn.fossil add comment '{"status":"open","content":"This is a second ticket added to this repo"}'
fossil ticket -R learn.fossil add comment '{"status":"open","content":"This is a third ticket added to this repo"}'
fossil sql -R learn.fossil 'select * from ticket'
fossil ticket -R learn.fossil set [uuid of the third ticket] comment '{"status":"closed","content":"This is a third ticket added to this repo"}'
fossil sql -R learn.fossil "select json_extract(ticket.comment, '$.content') from ticket where json_extract(ticket.comment, '$.status')='open'"
```

More to come...
