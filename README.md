# A ready-to-run client for the CENO network

See <https://censorship.no/>.

A CENO client has three main roles:

 1. Allowing a user to retrieve potentially censored content from injectors or other users.
 2. Sharing retrieved content with other users (seeding).
 3. Allowing other users to reach injectors (bridging).

## Building (developers only)

Build for the latest version of Ouinet with:

    $ sudo docker build -t equalitie/ceno-client .

Build for Ouinet version `vX.Y.Z` as `latest` with:

    $ sudo docker build --build-arg OUINET_VERSION=vX.Y.Z -t equalitie/ceno-client:vX.Y.Z -t equalitie/ceno-client:latest .

## Running the client

On a GNU/Linux system, run the client for the first time with:

    $ sudo docker run --name ceno-client -dv ceno:/var/opt/ouinet --network host --restart unless-stopped equalitie/ceno-client

This will start the client automatically along with the system.

If your system is not GNU/Linux (e.g. Windows, macOS), you will need to setup some port redirections as the client can not have direct access to the host's network stack. Replace `--network host` with `-p 127.0.0.1:8077-8078:8077-8078 -p 28729:28729/udp` in the previous command line.

Watch the client's output with:

    $ sudo docker logs -ft ceno-client

Subsequently, start/stop the client manually with:

    $ sudo docker start ceno-client
    $ sudo docker stop ceno-client

## Using the client as a bridge

A bridge client is nothing but a normal client (that can be run by anyone) which happens to:

  - Be able to reach injectors. This means that the ISP used by the device running the client is not blocking access to those particular Internet hosts.
  - Be reachable to other clients. As the device where the client runs may not have a public IP address, but its router may, the client tries to use UPnP to automatically set up the appropriate port redirections at the router; otherwise manual redirections can be set up by a user with admin access to the router.

Please note that many mobile connections (and some domestic ones) use CGNAT which makes the client unreachable from the outside, thus making them unfit for bridges.

Also note that if you run the client on a non-GNU/Linux system, you will need to manually setup a redirection to UDP port 28729 of the device running the client.
