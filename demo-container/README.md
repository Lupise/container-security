```bash
alias docker-demo='[[ ! "$(sudo docker ps -a -f name=controler-demo | grep controler-demo)" ]] && sudo docker run -it --rm --name controler-demo -h myhostname --net=host --pid=host -v ~/.kube/config:/home/user/.kube/config -v ~/.kube/:/home/user/.kube/ -v /var/run/docker.sock:/var/run/docker.sock --add-host=myhostname:127.0.0.1 booyaabes/demo /bin/bash || sudo docker exec -it controler-demo /bin/bash'
```
