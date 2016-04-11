---
layout: post
title: "[HPMYSQL] Chap 1. Mysql Logical Architecture"
date: 2016-04-11
categories: ruby
---

![logical view mysql architecture](/images/chap1.1.jpeg)

I. Cấu trúc logic của DB Mysql:

  - Tầng services: bao gồm connection handling, authentication, security khi connect tới DB Mysql (mysql2, jdbc, odbc, pg). Mỗi client sẽ có 1 luồng làm việc với server mysql, mỗi luồng khi connect thì cần authenticate thông qua username, password, Và mysql server sẽ caching luồng này => mysql server không tạo mới hoặc destroy connect cho những kết nối mới từ client này.
  
  - Tầng branning: code cho việc truy vấn query, analytis, optimizing, caching data and tất cả những built-in function (data, time, math, encrytion) and những function đụng tới storage engine như stored procedures, triggers, lambda, views. Mysql parser câu query để tạo nên internal structure dạng tree sau đó áp dụng nhiều loại optimize. Chúng có thể viết lại câu query sau đó sẽ đọc tables, chọn loại indexes để sủ dụng và truy vấn. 
  
  - Tầng storage engine: tầng này thì có trách nhiệm chính trong việc lưu trữ và tiếp nhận dữ liệu.

II. Concurrency Control - điều khiển truy cập đồng thời.

  - Read locks và Write locks

    Bất cứ khi nào, trong cùng 1 thời gian, thì sẽ luôn có nhiều request yêu cầu change data, đây là vấn đề về điều khiển đồng thời.
    
    MySql làm việc này ở 2 level: server level và storage engine level. Concurrency Control là 1 vấn đề lớn, ở đây chỉ overview về cách MySql làm việc về vấn đề này.
    
    Để giải quyết vấn đề cổ điển này, Hệ thống sẽ sử dụng việc điều khiển khóa, và được hiện thực trong  hệ thống khóa locking system, `shared lock` `exclusive lock` `read lock` và `write lock`. Cơ chế, `read lock` thì mang tính chia sẽ, tại mỗi thời điểm bất kể client nào cũng có khóa read lock để có thể read 1 resource at the same time. Còn với `write lock` là khóa độc quyền, 1 khi client đang có khóa `write lock` thì client này có cả `read lock` và những khóa `write lock` khác vì 1 client khi đang edit 1 resource tại 1 thời điểm thì mysql server sẽ ngăn chặn tất cả việc đọc resource đó đối với những client khác trong khi client này đang writing. Tức là khóa lại hết khi đang edit hoặc update resource (row data, resource data..)
    
  - Lock Granularity - Bản chất khóa
    
    Một điều quan trọng trong việc cải thiện chia sẽ resource là việc lựa chọn cái bạn khóa. Việc khóa cả một thực thể lớn sẽ không hiệu quả ảnh hưởng performance, nên ta chỉ cần khóa 1 phần của data mà ta đang muốn thay đổi. Khóa đúng thành phần dữ liệu mà ta muốn thay đổi (best practices). Tối thiểu hóa lượng dữ liệu mà bạn khóa. 

    Với mỗi lock operator, ta phải getting lock, checking lock for free use, releasing lock, and so on -> điều này dễ dẫn đến overrhead. Vấn đề là quản lý lock mà không chú trọng vào công việc chính của DB thì performance giảm.
    
    Mysql có 2 cách lock cơ bản là `table locks` và `row locks`
    
    `table locks` là khóa table khi  client muốn thực thi trên table ( write, delete, update) thì table này sẽ được lock lại, và các client khác lúc này sẽ không có quyền read và write table này, cho đến khi table này được mở khóa, nhắm tránh gây conflict. Quyền write có độ ưu tiên cao hơn quyền read, nên nếu request nào có quyền write thì được ưu tiên thực thi trước.
    
    `row locks` là khóa trên dòng dữ liệu, thường dùng với engine là  InnoDB, và được xây dựng trong engine DB.
    
III. Transaction - Engine
  
  - Deadlock
  
    `deadlock` xảy ra là khi 2 hay nhiều giao dịch cùng nắm giữ lẫn nhau, yêu cầu trên cùng 1 tài nguyên, phụ thuộc nhau, tạo ra 1 chu kỳ phụ thuộc tiếp diễn. 

  - Engine
    
    Thường chúng ta có 2 engines : InnoDB và MyISAM

    InnoDB là engine thông dụng nhất còn MyISAM có những tính năng về indexing và fulltext search.
    
  - Chọn engine đúng: Chúng ta nên chọn engines dựa trên các tiêu chí sau (mục đích sử dụng):

      - Transactions, Backups, Crassh recovery, Larger data: InnoDB.
      
      - Special features: MyISAM
