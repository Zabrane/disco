Building
========
In order to build a docker disco image you need to
[install docker](https://www.docker.io/gettingstarted/). Then go to
`disco/contrib/docker` and build the disco image:

    $ docker build -t disco/disco .

This command will fetch Ubuntu images from a public docker index and
build an image called `disco/disco`.

Running
========
You can run the disco image as follows:

    $ docker run -p 8989:8989 --name disco -ti disco/disco

Now disco is running inside the docker and you can see the web interface.

An SSH server is running on this image, and can be exposed by mapping it to
a host port:

    $ docker run -p 8989:8989 -p 2200:22 --name disco -ti disco/disco

The default root password is `'disco'`.

Using:
========
From the docker host you can run a terminal in the container as dsico (application) user by: 

    $ docker exec -it -udisco disco bash

Or as the root user by omitting the -udisco flag.
Inside the container, you can run examples with python like this:

    $ python /disco/examples/util/count_words.py 


Other option is to ssh into the container from the docker host as follows:

    $ ssh -l root -p 2200

And now you can run map-reduce jobs.

For more information about docker please visit [docker](https://www.docker.io/).
