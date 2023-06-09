Running the flask application 

1. launch ec2  and connect to ec2

        ssh -i <pemfile> ec2-user@<publicIp> 

2. install dependencies 

        yum install python3 
        yum install python3-pip 
        pip install flask 
        yum install git 

3. clone this repo

        git clone https://github.com/kalpanaIronbanda/python-flask.git

4. navigate to the project directory
   
        cd python-flask


5. run the application 

        python3 flask.py

This is manuall process 

For automation jenkinsfile will be helpful 

1. create an ec2 and set up jenkins 

2. In the jenkins create job with pipeline type

3. choose this url and give the jenkinsfile 

4. automatically it will do ci cd process for this flask application
