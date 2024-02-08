# Level 11 - Level 12  

### Challnge này giúp chúng ta hiểu cách dịch(position) các bảng chữ cái như mã caesar 

> Đề tài là đoạn mã kia đã được mã theo ROT13 (dịch 13 đơn vị theo alpha bet )

 
```
cú pháp  :  tr set1  set2 
set 1  :  là những ký tự sẽ bị thay thế 
set 2  :  những kí tự thay thế cho set 1 

```

```
strings data.txt | tr  'A-Za-z' 'N-ZA-Mn-za-m'
```
A-Z :  sẽ được thay thế ( chia ra làm 2 bởi vì ROT13 , nửa đầu của A-Z là A-M sẽ được thay thế bởi N-Z , nửa sau là N-Z vì tính đối xứng nên lại là A-M )

# FLAG 
```
6zPeziLdR2RKNdNYFNb6nVCKzphlXHBM
```