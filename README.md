Only for a reminder. Do not use it!! ;)

#Start local Nexus/Jenkins

docker-compose up

#Configure local Jenkins

Log into jenkins : http://localhost:8080/

##To know the password, 
In a terminal, ```docker exec -t -i c8866d6ee377 bash``` 

```more /var/jenkins_home/secrets/initialAdminPassword```

##Configure maven and jdk
go to ```Administrate Jenkins``` > ```Configure global tools```

add jdk with name ```jdk1.8u112```

add maven with name ```mvn-3.3.9```

##Force ugly git config, 
In a terminal, ```docker exec -t -i c8866d6ee377 bash``` 

```git config --global user.name "Your Name"```
```git config --global user.email "you@example.com"```

##Configure deploy user into nexus
Log in nexus : http://localhost:8081/nexus/

By default : admin/admin123
 
go into ```security``` > ```Users```. Add to anonymous : ```Nexus deployment role``` 
 




