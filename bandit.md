
# Bandit Write-up (Dịch sang tiếng Việt, giữ nguyên toàn bộ dữ liệu & hint)

## level 0 :

```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
```
- Sử dụng ls để liệt kê các file và thấy một file tên readme và dùng lệnh cat để đọc file đó và lấy pass

```
ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If
```
---
## level 1 :

```bash
ssh bandit1@bandit.labs.overthewire.org -p 2220
```

Sử dụng **ls -apls** để dò các file và có một file tên là '-', dùng lệnh cat để cat file đấy và ra được pass

```
263JGJPfgU6LtdEvgfWU1XP5yac29mFx
```
---
## level 2:

```bash
ssh bandit2@bandit.labs.overthewire.org -p 2220
```
- Tiếp tục dùng **ls -apls** để liệt kê toàn bộ file và thư mục và thấy một file có tên là "--spaces in this filename--"
- Để mở file có khoảng trắng trong tên thì dùng ký tự "\"

```
cat ./--spaces\ in\ this\ filename--
```
Và ra pass
```
MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx
```
---
## level 3:

```bash
ssh bandit3@bandit.labs.overthewire.org -p 2220
```
- Sử dùng lệnh **ls -apls** ra được một thư mục có tên là "inhere". Tiếp theo sử dụng cd để chuyển vào thư mục "inhere" tiếp theo ls -a thì ra file chứa mật khẩu là ...Hiding-From-You, chỉ cần cat để xem
```bash
cat ./...Hiding-From-You
```
Và ra được pass:
```
2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ
```

## level 4:

```bash
ssh bandit4@bandit.labs.overthewire.org -p 2220"
```

- File chứa mật khẩu nằm trong thư mục inhere nên cd vào inhere.
- Sau khi chuyển từ ~ sang inhere thì có 10 file, chỉ có 1 file đọc được và đó là mật khẩu
- Các cách có thể dùng:
+ way 1: brute-force bằng cách cat toàn bộ file  
+ way 2: dùng find:
```bash
find -type f | xargs file
# hoặc
find . -type f -exec file {} +
```
- Cuối cùng thì chỉ cần vào file đó và cat là ra được pass
```
4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw
```
## level 5:

```bash
ssh bandit5@bandit.labs.overthewire.org -p 2220
```
Nhiệm vụ: Cần tìm file có các điều kiện: human-readable, kích thước 1033 bytes, không executable.
- Dùng lệnh find với các điều kiện:
```
find . -type f ! -executable -readable -size 1033c
```
- Sau khi tìm được thư mục chứa file thì cat file đó
```
HWasnPhtq9AVKe0dmk45nxy20cvUa6EG
```
## level 6:
```bash
ssh bandit6@bandit.labs.overthewire.org -p 2220
```
Giống level 5 nhưng điều kiện là:
- thuộc user bandit7
- thuộc group bandit6
- kích thước 33 bytes
  
-> dùng lại lệnh level 5 nhưng đổi "." thành "/" (root)

+ file chứa mật khẩu: /var/lib/dpkg/info/bandit7.password

cat file đó và ra được pass
```
morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj
```

## level 7:
```bash
ssh bandit7@bandit.labs.overthewire.org -p 2220
```
- Mật khẩu nằm trong file data.txt, đứng cạnh từ "millionth"
- 
--> dùng grep
```bash
  cat data.txt | grep "millionth"
```
Pass:
```
dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc
```

## level 8:

+ "ssh bandit8@bandit.labs.overthewire.org -p 2220"
+ cần in ra dòng chỉ xuất hiện duy nhất 1 lần
--> sort file rồi dùng uniq:
==> sort data.txt | uniq -u

+ pass: 4CKMh1JI91bUIZZPXDqGanal4xvAg0JM

## level 9:

+ "ssh bandit9@bandit.labs.overthewire.org -p 2220"
+ không thể dùng cat vì file là binary
--> dùng strings rồi grep dòng có dấu "="
==> strings data.txt | grep =

+ pass: FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey

## level 10:

+ "ssh bandit10@bandit.labs.overthewire.org -p 2220"
+ mật khẩu được encode base64
--> dùng strings rồi decode:
==> strings data.txt | base64 -d

+ pass: dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr

## level 11:

+ "ssh bandit11@bandit.labs.overthewire.org -p 2220"
+ mật khẩu bị mã hóa ROT13
+ không có lệnh rot13 nên dùng tr
+ bảng ROT13:
A-Z -> N-ZA-M  
a-z -> n-za-m

+ pass: 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4

## level 12:

+ "ssh bandit12@bandit.labs.overthewire.org -p 2220"
+ data.txt là hex dump
+ dùng mktemp -d tạo thư mục tạm
+ dùng xxd -r để chuyển về binary
+ dùng file để xác định các lớp nén:
gzip → bzip2 → tar
+ giải nén từng lớp đến khi ra file text
--> cat để lấy mật khẩu

+ pass: FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn

## level 13:

+ "ssh bandit13@bandit.labs.overthewire.org -p 2220"
+ không cần tìm mật khẩu
+ dùng ssh key để login bandit14
+ cat sshkey.private, copy nội dung
+ lưu vào file .key, chmod 600
+ ssh vào bandit14

## level 14:

+ gửi mật khẩu level trước cho bandit14
+ cat /etc/bandit_pass/bandit14
+ dùng netcat
hint: nc host port

+ pass: 8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo

## level 15:

+ dùng mật khẩu bandit14
+ connect localhost port 30001
+ dùng openssl s_client
hint: openssl s_client -connect IP_host:port

+ pass: kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx

## level 16:

+ dùng kiến thức ssh key level 13
+ scan port từ 31000-32000
+ dùng nmap để tìm port mở
+ dùng openssl s_client kết nối
hint: nmap -p <port-range> IP_host
ports: 31790, 31518

## level 17:

+ dùng RSA key level 16
+ so sánh password.new và password.old
hint: diff file1 file2

+ pass: x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO

## level 18:

+ login bandit18 sẽ auto logout
+ cần đọc readme
+ ssh kèm lệnh "cat readme"

+ pass: cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8

## level 19:

+ "ssh bandit19@bandit.labs.overthewire.org -p 2220"
+ file bandit20-do có setuid (s)
+ setuid cho phép chạy file với quyền owner (bandit20)
+ bandit19 có thể thực thi command với quyền bandit20

+ pass: 0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO

## level 20:

+ "ssh bandit20@bandit.labs.overthewire.org -p 2220"
+ suconnect kết nối TCP tới port chỉ định
+ tạo server bằng netcat:
nc -l -p port
+ suconnect gửi password bandit20

+ pass: EeoULMCra2q0dSkYj561DX7s1CpBuOBt

## level 21:

+ "ssh bandit21@bandit.labs.overthewire.org -p 2220"
+ kiểm tra /etc/cron.d/
+ cronjob_bandit22 chạy script
+ script ghi password vào /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv

+ pass: tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q

## level 22:

```bash
ssh bandit22@bandit.labs.overthewire.org -p 2220
```
- Sử dụng trick cũ của bandit21 vào file **/etc/cron.d**
- cat file **cronjob_bandit23** thì ra được đoạn mã bash như này:
```bash

```
+ script dùng md5(username) làm tên file
+ hash:
8ca319486bfbbc3663ea0fbe81326349
+ cat /tmp/hash

+ pass: 0Zf11ioIjMVN551jX3CmStKLYqjk54Ga

## level 23:

+ "ssh bandit23@bandit.labs.overthewire.org -p 2220"
+ cronjob_bandit24
+ script chạy mọi file trong /var/spool/bandit23/foo
+ file do bandit23 sở hữu sẽ được chạy với quyền bandit24

