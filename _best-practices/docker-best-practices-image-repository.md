Image Repository
================

Community images
----------------

After you’ve constructed your Docker images, one of the main advantages
that Docker brings is focused on the fact that the community has created
several Docker images for a slew of purposes. Many of the images created
by users have been uploaded to the Docker Hub Registry
(*https://registry.hub.docker.com/*) and this allows everyone to use
them.

You can retrieve or pull an image by identifying the
\<user/image\_name\> and passing it to Docker, i.e: \` docker pull
cpuguy83/openvpn\`

The Docker Hub Registry also has official images from certain
providers ranging from OS companies to software providers i.e. Ubuntu,
MySQL, Golang etc. Pulling these images has a shortened call such as
\`docker pull mysql\`.

Personal images
---------------

Of course, you too can upload your custom images to the Docker Hub
Registry, CoreOS’ Quay.io or to your own private image registry.
Pushing an image to the Docker Hub Registry is as simple as issuing
\`docker push \<user/image\_name\>\` after you’ve logged in using \`docker
login\`. You can also tag the image if you wish to give it a label
i.e. dev, production, v2 etc. to distinguish amongst various flavors
of your image.

The default behavior for \`docker push\` is to push to the central
Docker Hub Registry, but in the case that you prefer your own private
Docker registry there is an image for that too. Once you’ve got it
running, the only step that you are required to change is to login to
your private registry as opposed to the central one. That switch is as
easy as \`docker login http(s)://\<YOUR\_DOMAIN\>:\<PORT\>\`.
