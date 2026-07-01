# Module 5 - Cloud & Infrastructure as Service Basics

## Demo Project for DevOps Bootcamp by [TechWorld with Nana](https://www.techworld-with-nana.com/):
Create server and deploy application on DigitalOcean

## Technologies used:
- DigitalOcean
- Linux
- Java
- Gradle

## Project Description
- Setup and configure a server on DigitalOcean
- Create and configure a new Linux user on the Droplet (Security best practice)
- Deploy and run a Java Gradle application on Droplet

## Implementation Steps:
1. Login to DigitalOcean account (the account was already created in a previous module)
2. Navigate in the menu sidebar to COMPUTE -> Droplets
3. Click 'Create Droplet' button
4. Choose data center region next to your location
5. Choose settings for new Droplet (Ubuntu for OS, for the rest choose cheapest options)
6. Add SSH key
   1. Navigate on your local machine to .ssh directory in your home folder
   2. Copy content of public ssh key `id_rsa.pub`
   3. Click at 'Add SSH Key' button at DigitalOcean
   4. Paste and give name to public key
7. Click 'Create Droplet' button and wait for startup
8. Switch in Droplet UI to Networking tab
9. Click 'Manage Cloud Firewalls' button
10. Click 'Create Firewall' button
11. Inbound rule for SSH (port 22) already exists per default.
12. To secure the server, replace existing sources in the rule by your own IP address
    1. Determin your public IP address e.g. at [WhatIsMyIpAddress](https://whatwsmyipaddress.com)
    2. Replace sources 'All IPv4' and 'All IPv6' with your public IP
13. Choose the newly created Droplet in the 'Apply to Droplets' field
14. Click 'Create Firewall' button
15. SSH into the server with `ssh root@<public-droplet-ip>`. Since we added the SSH key, no password is required
16. Update apt with `apt update`
17. Install Java with `apt install openjdk-17-jre-headless` (Java 17 is required for compatibility with Gradle)
18. Clone [demo project](https://gitlab.com/twn-devops-bootcamp/latest/05-cloud/java-react-example) on local machine
19. Build demo project with `gradle build`
20. Copy artifact with `scp build/libs/java-react-example.jar root@<public-droplet-ip>:/root` to home folder of root user on the server
21. Start applicaton with `java -jar java-react-example.jar` from `/root` directory
22. Output shows that application is listening on port 7071. To make it accessible, create another inbound firewall rule for that port
23. Open local browser at `<public-droplet-ip>:7071` to verfiy that the application is reachable and working
24. Since it's a bad practice to work with root user, create another user on the system
    1. Create new user with `adduser <username>` and set a password
    2. Add new user to sudo group with `usermod -aG sudo <username>` for privilege escalation
    3. Switch to new user with `su - <username>`
    4. In new users home directory create directory `.ssh` and in that the file `authorized_keys`
    5. Copy public SSH key from socal machine to the `authorized_keys` file. With that SSH login to new user from local machine is possible
