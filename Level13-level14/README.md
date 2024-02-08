# Level 13 -> Level 14

### Challnge này giúp mình hiểu về đăng nhập SSH bằng private key 

>  Mã hóa bất đối xứng , public key được lưu ở server (mọi người đều xem được ) , mình giữ private key , đăng nhập cùng nó thay cho password 

```
cú pháp  :
lưu ý mình phải lưu cái private key của người ta lại  
chmod 700 priavte.key  

ssh -p 2220 bandit14@bandit.labs.overthewire.org -i private.key 
```

# Flag 
```
fGrHPx402xGC7U7rXKDaxiWFTOiF0ENq
```