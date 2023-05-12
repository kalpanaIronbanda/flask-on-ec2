#create simple flask code and push it in the scm 

1.launch ec2 ssh to ec2

2.ssh -i pemfile ec2-user@publicIp 

3.install dependencies 
a.yum install python3 
b.yum install python3-pip 
c.pip install flask 
d.yum install git 

4.clone the code from remote to ec2 
5.run the application 
>>python3 flask.py