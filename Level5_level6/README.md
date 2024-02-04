# Level 5 -> Level 6 


## Challenge này giúp mình tìm kiếm file theo yêu cầu 

Đề bài yêu cầu tìm 1 file ( chứa pass của bandit6) theo 3 yêu cầu 
+ human readable 
+ 1033 bytes in size 
+ not executable 

Lệnh thực hiện được các yêu cầu trên chính là lệnh **find**

```
cú pháp : find [path] [condition] [action]

+ Điều kiện : 
   -name "pattern" :  tìm kiếm theo tên regex
   -type type  :  tìm kiếm theo loại file : f(file) ,d(directory)
   -size n[c]  : tìm kiếm theo byte (c là chỉ số byte) ,+ là lớn hơn , - là nhỏ hơn

+ Hành động : 
  '-exec command {} +'

phủ định thì thêm dấu ! 
```

Từ lí thuyết trên kết hợp các điều kiện 

```
find . -type f -size 1033c -readable ! -executable

. :  thể hiện thư mục hiện tại 
-type f :  chỉ là type là file 
-size :  tìm kiếm theo kích thước 
-reaable :  con người có thể đọc được 
! - executable :  ko thể thực thi
```

Dùng pipeline để thực hiện đọc 
```
find . -type f -size 1033c -readable ! -executable | xargs cat 

xargs :  execute argument :  thực hiện chuyển đổi số sang cho lệnh mới
```

# FLAG 
``` 
P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU
```