# Level 8 -Level 9 

## Challenge này giúp chúng ta đếm số lần xuất hiện của từng dòng 

thông qua các lệnh **sort, uniq**
```
Lệnh sort 
Cú pháp :  sort name.txt

option  : 
 -n :  theo number 
 -r :  revesr
 -u : loại bỏ trùng lặp ( mặc định là sắp xếp theo alphabet)
 -k2 : theo cột 2


 Lệnh uniq : 
 => Bỏ các dòng trùng nhau ( nằm cạnh nhau)
 Cú pháp :   uniq name.txt 

 Option  : 
 -c : đếm số lần xuất hiện của mỗi dòng 
 -d :  chỉ xuất những dòng nhiều hơn 1 lần 


```

=> từ các lí thuyết trên payload là 
```
sort data.txt | uniq -c | grep -w 1 

-w : word : ko phải là 1 thành phần trong từ 
phải sort để cho các dòng giống nhau nằm cạnh nhau để uniq xóa :>
```

# FLAG 
```
EN632PlfYiZbn3PhVK3XOGSlNInNE00t
```