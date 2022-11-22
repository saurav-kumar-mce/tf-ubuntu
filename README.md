# tf-ubuntu\


#!/bin/bash
yum update -y
yum install start httpd.x86_64
systemctl start httpd.service
systemctl enable httpd.service
echo "The page was created by saurav for the user data test" | sudo tee /var/www/html/index.html

resource "aws_instance" "web"{
    ami = "ami-0de5311b2a443fb89"
    instance_type = "t2.micro"
    key_name = "Demo1"
   security_groups = ["${aws_security_group.TF_SG.name}"]
   
    
    

    tags = {
      Name = "hello-saurav"
    }


    user_data = file("script.sh")

}




resource "aws_security_group" "TF_SG" {
    name = "saurav"
    description = "saurav"
    vpc_id = "vpc-03011000d8048f821"
  


 ingress {
    description      = "HTTP"
    from_port        = 80
    to_port          = 80
    protocol         = "tcp"
    cidr_blocks      = ["0.0.0.0/0"]
    ipv6_cidr_blocks = ["::/0"]
  }

   ingress {
    description      = "HTTPS"
    from_port        = 443
    to_port          = 443
    protocol         = "tcp"
    cidr_blocks      = ["0.0.0.0/0"]
    ipv6_cidr_blocks = ["::/0"]
  }

   ingress {
    description      = "SSH"
    from_port        = 22
    to_port          = 22
    protocol         = "tcp"
    cidr_blocks      = ["0.0.0.0/0"]
    ipv6_cidr_blocks = ["::/0"]
  }
  
    egress {
    from_port        = 0
    to_port          = 0
    protocol         = "-1"
    cidr_blocks      = ["0.0.0.0/0"]
    ipv6_cidr_blocks = ["::/0"]
    
  }

  tags = {
    Name = "TF_SG"

  }
}
