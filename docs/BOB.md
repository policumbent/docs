# BOB

## Introduzione

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vestibulum sollicitudin nunc vitae tincidunt lobortis. Sed erat lorem, elementum sed dapibus eu, gravida a sapien. Fusce condimentum libero et auctor tempus. Nunc ut lobortis risus. Aenean aliquet ipsum non molestie molestie. Praesent fringilla nunc eget ipsum tincidunt elementum. Sed ultricies finibus sapien, vel consectetur est feugiat ut. Aliquam erat volutpat.

## Installazione

1. Installare git `sudo apt install git`
2. Scaricare BOB `git clone https://github.com/policumbent/BOB.git`
3. Installare docker-compose `sudo apt install docker-compose`
4. Installare portainer (facoltativo => per un più facile controllo dei container)
   1. `sudo docker volume create portainer_data`
   2. `sudo  docker run -d -p 8000:8000 -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce`
5. Copiare i fle in comune `python3 copy_common.py`
6. Buildare e avviare i container `sudo docker-compose up -d`

## Moduli

Praesent maximus imperdiet elit non faucibus. Nullam eget feugiat orci. Curabitur vel maximus ipsum. Pellentesque pulvinar lacus neque, eu blandit nisi ornare eget. Curabitur ultricies, quam efficitur gravida ultricies, eros dui vestibulum dolor, sit amet egestas elit sem a tortor. Phasellus porta, augue et lacinia pretium, nisl nunc cursus dolor, eget facilisis odio ligula quis lectus. Donec tempus et mi id tempor. Vivamus sit amet dui sit amet nisi iaculis elementum. Pellentesque nisl lacus, aliquam eget ipsum volutpat, fermentum semper quam. Curabitur tempor dictum odio id ultrices. Duis tincidunt efficitur metus, non consequat ex volutpat quis. Curabitur semper nulla ac ipsum accumsan tempor. Proin maximus arcu ac bibendum imperdiet. Donec sed ultricies metus.

### Gps

Nam condimentum aliquam neque at dignissim. Praesent suscipit massa a elit congue mollis. Interdum et malesuada fames ac ante ipsum primis in faucibus. Aenean sed iaculis metus. Integer tempor augue nec porta feugiat. Nulla eleifend sed dui quis maximus. Sed rutrum fermentum orci at hendrerit. Sed suscipit tempor massa, eget pellentesque tellus sagittis a. Pellentesque dapibus gravida sem, sit amet cursus lacus eleifend ac. Nullam lobortis tristique nisl, id pulvinar libero molestie vitae. Donec porttitor ultricies dui, sed pretium libero fringilla et. Sed dictum blandit enim, vel euismod magna viverra at. Integer mi velit, iaculis ornare porta at, placerat vel nibh. Phasellus dictum, sem eget tempus ultricies, enim turpis dignissim ante, sit amet porta sapien tortor at ligula.

### Ant

Phasellus porta sit amet magna ut rhoncus. Fusce congue purus lacus, in facilisis enim posuere ut. Nulla fermentum leo mi, sit amet consequat metus placerat sit amet. Sed pellentesque in dui et pharetra. Donec non mauris at nulla viverra posuere eget id purus. Aliquam fringilla tincidunt ante eu congue. Aliquam erat volutpat. Donec id venenatis leo. Phasellus vitae consectetur nisi. Fusce dictum consectetur ex eget placerat.

### Communication

Phasellus porta sit amet magna ut rhoncus. Fusce congue purus lacus, in facilisis enim posuere ut. Nulla fermentum leo mi, sit amet consequat metus placerat sit amet. Sed pellentesque in dui et pharetra. Donec non mauris at nulla viverra posuere eget id purus. Aliquam fringilla tincidunt ante eu congue. Aliquam erat volutpat. Donec id venenatis leo. Phasellus vitae consectetur nisi. Fusce dictum consectetur ex eget placerat.
