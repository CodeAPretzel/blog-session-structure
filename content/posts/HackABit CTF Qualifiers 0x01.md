+++
title = "HackABit CTF Qualifiers 0x01 - WriteUp"
title_img = "" 
date = "2023-09-04T18:42:23-05:00" 
author = "CodeAPretzel" 
authorTwitter = "PretzelMano" #do not include @ 
cover = ""
tags = ["ctf", "infrastructure", "writeup", "beginners"] 
keywords = [""] 
description = "A writeup of the main challenge category, infrastructure. Over these challenges, we will see their vulnerabilities." 
showFullContent = false 
readingTime = false 
hideComments = false 
color = "" #color from the theme settings
+++

# 💥 Hack-A-Bit 0x01 💥

<br>

In the Hack-a-Bit (HaB) category infastructure, you will find the following challenges with their designated points:


  1. - Captain (75 points)
  2. - Seashell (75 points)
  3. - Loading (100 points)
  4. - Location (100 points)
  5. - Connector (125 points)
  6. - Boat (125 points)
  7. - Inspector (150 points)
  8. - Hammer (150 points)

With each challenge we will figure out the problems and how to solve each one:

<hr>
<br>

## - Captain 🌊 -
**Challenge**: Cloud infrastructure is migrating to containerized technology in many places, lets get started with containers. Run this docker container with the environment variable FLAG_ENABLER set to give_me_the_flag.

**Container**: https://hub.docker.com/r/nathanielsinger/hackabit0x01-infrastructure-container1

|
<br>

**Solution**: The point of this challenge is for the challenger to find a way to run the container with defining the set variable in the container. For this challenge you can use any updated distro of Linux. Before we could use the pull request in URL given, we first need to install the Docker packages necessary using the command line below:

<br>

```
  sudo apt-get update
  sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

<br>

After that we pull the container from the URL:

<br>

```
  docker pull nathanielsinger/hackabit0x01-infrastructure-container1
```

<br>

Afterwards, all we need to do is set the ENV. variable for the container which, after a google search later, we get the following command:

<br>

```
  sudo docker run -e FLAG_ENABLER=give_me_the_flag nathanielsinger/hackabit0x01-infrastructure-container1 #
```

<br>

This brings the output: **flag{you_aren't_the_captain_just_yet}**

<br>
<br>

## - Seashell 🐚 -
**Challenge**: SSH runs the internet. Connect to utkwrgubhj.qualifier.hackabit.com on port 22 with user hackerman and the provided private key.

**Key**: <a href="https://github.com/CodeAPretzel/Hack-A-Bit-2023/blob/main/Assets/id_rsa">Go to File</a>

|
<br>

**Solution**: The point of this challenge is for the challenger to learn about connections with private and public keys and how to gain access into a session using one of the keys. To begin with, after downloading the key, we are told in the challenge that we will have to use SSH (secure socket shell) on port 22, which SSH runs on default. In order to use a key with SSH for a session we use the -i parameter for the following command:

<br>

```
  ssh -i id_rsa hackerman@utkwrgubhj.qualifier.hackabit.com  #make sure you go to the directory where you saved your file
```

<br>

However, the session doesn't accept this because the key has too many accessible permissions. To reduce the permissions needed for the session to just read the key, we will use the command bellow:

<br>

```
  chmod 400 id_rsa
```

<br>

After this we will just run the first command and get the output: **flag{shesellsseashellsbytheseaaaaaaashore}**

<br>

*Side Note: I tried running these commands on the actual session but it appeared to be down.*

<br>
<br>

## - Loading 🌀 -
**Challenge**: What's in this ISO thing?

|
<br>

**Solution**: With this challenge we are given the same Docker container from the Captain challenge. With no clue on what the challenge has in store, let's see what information we can get from extracting the image from the container and saving it to our system using the command below.

<br>

```
  docker save nathanielsinger/hackabit0x01-infrastructure-container1 > image.tar
```

<br>

Finally, just like a .zip file, we have to unpack the .tar file using the following command:

<br>

```
  tar -xvf image.tar
```

<br>

Afterward, go to the directory you're in and find the folder 89552...ea423 and the file layer.tar. Unpack the .tar `tar -xvf layer.tar`
<br>
With that, you go from `root > flag_image.iso` then `cat FLAG.TXT`

<br>

Getting the output: **flag{consider_it_loaded_hackerman}**

<br>

<a>
  <img src="https://github.com/CodeAPretzel/Hack-A-Bit-2023/blob/main/Assets/picLoading.png" alt="Img Loading" width="50%" height="50%">
</a>

<br>
<br>

## - Location 🌎 -
**Challenge**: Now with the same image, dive on in and find the iso image. What is the absolute path to the iso image stored within the container? Include the filename in the path for your submission.

|
<br>

**Solution**: This one is pretty simple, the path of the file is just the location in the container. If we go back to the folder `root` from layer.tar to `flag_image.iso`, it would be something like `/root/flag_image.iso`

<br>
<br>

## - Connector 🔌 -
**Challenge**: Connect to the mysql server at dyxvqmjwaj.qualifier.hackabit.com and read out the flag. Here are some user accounts:

<ul>
  <li>user1:uyqhxgxcxd</li>
  <li>user2:ehaigdexhh</li>
  <li>user3:xfgyuvtapt</li>
  <li>user4:tnvgijqxei</li>
  <li>user5:hybplwmndy</li>
</ul>

|
<br>

**Solution**: For this challenge we need to go into the challenge's MySQL database, I used MySQL for this. With this challenge I installed it on Windows using this link <a href="https://dev.mysql.com/downloads/installer/" target="_blank">here</a>
After completing the installation process, we have to create a new MySQL Connection like the below images:

<br>

<a>
  <img src="https://github.com/CodeAPretzel/Hack-A-Bit-2023/blob/main/Assets/createConnection.png" alt="Img Connector 1" width="50%" height="50%">
</a>

<br>

Then we put the following information in the fields:

<br>

<a>
  <img src="https://github.com/CodeAPretzel/Hack-A-Bit-2023/blob/main/Assets/testConnection.png" alt="Img Connector 2" width="50%" height="50%">
</a>

<br>

With this we should connect to the server, however, it would appear that the server is actually down. Nevertheless, when going to each user and checking their tables, you'll eventually go to `user4` and find the table with the solution's flag.

<br>

Flag: **flag{oh_sql_my_sql}**

<br>
<br>

## - Boat ⛵ -
**Challenge**: Sometimes we need to run a machine on a specific address or virtualize a network, get this running on: 172.22.1.11.

**Container**: https://hub.docker.com/r/nathanielsinger/hackabit0x01-infrastructure-container2

|
<br>

**Solution**: This challenge just giving us a Docker container to run on an IP. First things first, grab the pull request from the URL run it and wait, we get an error.

<br>

<a>
  <img src="https://github.com/CodeAPretzel/Hack-A-Bit-2023/blob/main/Assets/containerError.png" alt="Img Boat" width="75%" height="75%">
</a>

<br>

Not to worry however, from this Docker discussion, <a href="https://forums.docker.com/t/docker-error-response-from-daemon-manifest-not-found-when-running-container-following-get-started-tutorial/65107">here</a>, we find out that an error response might be due to the fact that we have to specify the version of the container. In the form, it says that we can go to the container and under the header "tags" get the version we need.

<br>

```
 docker pull nathanielsinger/hackabit0x01-infrastructure-container2:v1.0.0
```

<br>

Now that we have the container, we need to figure out on how to set this to an IP. With some googling, we will find a Docker documentation for just this feature <a href="https://docs.docker.com/engine/reference/commandline/network_create/#:~:text=Connect%20containers&text=If%20you%20want%20to%20add,container's%20IP%20address%20or%20name.">here.</a>
Using the link above, we can create our own commands as such in the following image:

<br>

<a>
  <img src="https://github.com/CodeAPretzel/Hack-A-Bit-2023/blob/main/Assets/boatSuccess.png" alt="Success" width="50%" height="50%">
</a>

<br>

And you get the following flag: **flag{its_just_an_address_man}**


<br>
<br>

## - Inspector 🔍 -
**Challenge**: Oh look its Bits, something changed though... see if you can track it down.

**File**: <a href="https://github.com/CodeAPretzel/Hack-A-Bit-2023/blob/main/Assets/bits.zip">Go to File</a>

With this file, we are given the code documentation for the bot "Bits" in the HaB Discord server. At a first glance, it might be difficult to determine what the challenge is asking but looking into the challenge's material, <a href="https://qualifier.hackabit.com/learning/3n60u-inspector">here</a>, it appears that we are supposed to get a git version of Bits.
<br>
When looking through the files in Bits, we get to a file with the directory `C:\Users\$USER\Downloads\bits\bits\.git\logs\HEAD`. Inside of this file, we get the following commit hash `a0235bf2cf43159eff20f5b3d568a21ab288c9ff` with the description `flag_stuff`.

<br>

<a>
  <img src="https://github.com/CodeAPretzel/Hack-A-Bit-2023/blob/main/Assets/bitsHead.png" alt="Bits Head" width="75%" height="75%">
</a>

<br>

First, go to the directory where bits is stored, then use the command below to go to the committed version of `flag_stuff`.

<br>

```
  git checkout a0235bf2cf43159eff20f5b3d568a21ab288c9ff
```

<br>

After that we are given a file called "flag". Now we use this command to read the file:

<br>

```
  cat flag
```

<br>

Or this command:

<br>

```powershell
  grep -r "flag"
```

<br>

<a>
  <img src="https://github.com/CodeAPretzel/Hack-A-Bit-2023/blob/main/Assets/bitsOutput.png" alt="Bits Output" width="50%" height="50%">
</a>

Giving us the output: **flag{don't_try_harder...look_harder}**

<br>
<br>

## - Hammer 🔨 -
**Challenge**: Check out oslyxpzcgs.qualifier.hackabit.com and see if you can find the vuln. No help on this one, nothing crazy though... enumerate harder :)

The flag is stored in an environment variable.

With this challenge I actually didn't finish this. I would just do it right here, however, it would appear that the session is currently down, but what I've learned from this challenge is that it's a `vsftpd Smiley Face Backdoor` vulnerability. Apparently using the :) in the username results in shell listening on TCP port 6200.

<br>
<br>

You can learn more <a href="https://en.wikipedia.org/wiki/Vsftpd#:~:text=Users%20logging%20into%20a%20compromised,vsftpd%20which%20contained%20a%20backdoor.">here</a>.

~ CodeAPretzel
