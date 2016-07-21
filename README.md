# Chatroom
server1.py
  1 import socket
  2 import time
  3 import threading
  4 def sendsock(i,data):
  5     global user
  6     for key in user:
  7         if user.index(key)==i or key==1:
  8             continue
  9         key.send(data.encode('utf-8'))
 10 
 11 def tcplink(i,addr):
 12     global user
 13     print('Accept new connection from  %s:%s...\n'% addr)
 14     user[i].send(b'Welcome')
 15     while True:
 16         data = user[i].recv(1024).decode('utf-8')
 17 
 18         time.sleep(1)
 19         s=sock
 20         if not data or data =='exit' :
 21             break
 22         sendsock(i,data)
 23     user[i].close()
 24     user[i]=1
 25     print('Connection from %s:%s closed.'%addr)
 26 
 27 s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
 28 i=-1
 29 user=[]
 30 s.bind(('127.0.0.1',9000))
 31 s.listen(5)
 32 print('listen....')
 33 
 34 while True:
 35     global i
 36     i=i+1
 37     sock,addr=s.accept()
 38     user.append(sock)
 39     t=threading.Thread(target=tcplink,args=(i,addr))
 40 
 41     t.start()
 42 
 43 
client.py

1 import socket,threading
  2 def run_thread1(s,data):
  3     while True:
  4         data=input()
  5         s.send(data.encode('utf-8'))
  6 def run_thread2(s,data):
  7     while True:
  8         cdata=s.recv(1024).decode('utf-8')
  9         print('%s\n'%cdata)
 10 
 11 
 12 s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
 13 
 14 s.connect(('127.0.0.1',9000))
 15 data='Welcome!'
 16 t1=threading.Thread(target=run_thread1,args=(s,data))
 17 t2=threading.Thread(target=run_thread2,args=(s,data))
 18 t1.start()
 19 t2.start()
