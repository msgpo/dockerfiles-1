# dockerfiles

`docker` and `docker-compose` files I use every day

Docker is the best thing to happen to the open source world in a long time, if you aren't using it -- GET ON IT!!

## How can I use?

#### to run Dockerfiles that do not have a compose element:

1. build it!

    `sudo docker build -t $IMAGE_NAME .`

2. run it!

    `sudo docker run -d -rm --name $CONTAINER_NAME $IMAGE_NAME`

#### to run `docker-compose.yml` files

1. build it!

    `sudo docker-compose build`

2. run it!

    `sudo docker-compose up -d`


### Can I edit?

YES! If you have a problem with a file or would like to add something, open a PR / Issue!

### LICENSE

All are MIT unless otherwise stated inside their folder.
