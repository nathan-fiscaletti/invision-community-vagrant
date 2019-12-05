# Invision Community Vagrant Provisioner

This utility will let you provision a development environment for Invision Community.

## What is Invision Community?

Read more about Invision Community [here](https://invisioncommunity.com).

## How to use?

1. Clone this repository using `git clone https://github.com/nathan-fiscaletti/invision-community-vagrant.git`
2. Run `cd invision-community-vagrant` to change to the proper directory.
3. Download a copy of the Invision Community Suite from your [Invision Community Client Area](https://invisioncommunity.com/clientarea/)
4. Extract the software into the `ips` directory, once extracted you should now have the file `init.php` in the location `./ips/init.php`.
5. Open the `Vagrantfile` and configure the `EXPOSE_PORT` property to your liking. This will be the port that the software will listen on on your local machine.
6. Run `vagrant up` and wait for the process to complete. You will be presented with some instructions once it has completed.
7. You can now work on your code within the `ips` directory, while using the software through the URL provided by the `vagrant up` command.
