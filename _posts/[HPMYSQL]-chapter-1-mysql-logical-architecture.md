---
layout: post
title: "[HPMYSQL] Chap 1. Mysql Logical Architecture"
date: 2015-11-17
categories: ruby
---

![logical view mysql architecture](/images/chap1.1.jpeg)

I. Cấu trúc logic của DB Mysql:

  1. Tầng services: bao gồm connection handling, authentication, security khi connect tới DB Mysql (mysql2, jdbc, odbc, pg)
    - Mỗi client sẽ có 1 luồng làm việc với server mysql, mỗi luồng khi connect thì cần authenticate thông qua username, password, Và mysql server sẽ caching luồng này => mysql server không tạo mới hoặc destroy connect cho những kết nối mới từ client này.
  
  2. Tầng branning: code cho việc truy vấn query, analytis, optimizing, caching data and tất cả những built-in function (data, time, math, encrytion) and những function đụng tới storage engine như stored procedures, triggers, lambda, views.
  
    - Mysql parser câu query để tạo nên internal structure dạng tree sau đó áp dụng nhiều loại optimize. Chúng có thể viết lại câu query sau đó sẽ đọc tables, chọn loại indexes để sủ dụng và truy vấn. 
  
  3. Tầng storage engine: tầng này thì có trách nhiệm chính trong việc lưu trữ và tiếp nhận dữ liệu.

II. Concurrency Control - điều khiển truy cập đồng thời.

Bất cứ khi nào, trong cùng 1 thời gian, thì sẽ luôn có nhiều request yêu cầu change data, đây là vấn đề về điều khiển đồng thời.

MySql làm việc này ở 2 level: server level và storage engine level. Concurrency Control là 1 vấn đề lớn, ở đây chỉ overview về cách MySql làm việc về vấn đề này.

Để giải quyết vấn đề cổ điển này, Hệ thống sẽ sử dụng việc điều khiển khóa, và được hiện thực trong  hệ thống khóa locking system.
