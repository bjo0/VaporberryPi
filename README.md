# Introduction

This is a demo for getting Vapor running on a Raspberry Pi using swift 4.1.1 from the [swift-arm](https://swift-arm.com/install-swift/) port.

# Getting Started

## Develop on desktop, run on RaspberryPi

### Steps to do on your Raspberry Pi
1. Setup your Raspberry Pi with [Raspbian Stretch](https://www.raspberrypi.org/documentation/installation/installing-images/)
1. [Enable ssh](https://www.raspberrypi.org/documentation/remote-access/ssh/)
	* Set up [passwordless ssh access](https://www.raspberrypi.org/documentation/remote-access/ssh/passwordless.md) to avoid entering password
1. Install swift 4.1.1 from the [swift-arm](https://swift-arm.com/install-swift/) port
1. Clone a bare git repo, e.g. `sudo mkdir /git && sudo git clone --bare ssh://pi@raspberrypi.local/git/VaporberryPi.git /git/VaporberryPi.git && sudo chown -R pi:pi /git`
1. Add a post-receive hook script to your bare git repo that runs when you push new code to it (see post-receive hook example in Appendix)
1. Clone your bare git repo to create a working git repo, e.g. `git clone ssh://pi@localhost/git/VaporberryPi.git ~/VaporberryPi`

### Steps to do on your desktop computer
1. Clone VaporberryPi from the bare repo on your Raspberry Pi, e.g. `git clone ssh://pi@raspberrypi.local/git/VaporberryPi.git`
1. Change current directory to your VaporberryPi clone, e.g. `cd VaporberryPi`
1. Use ssh to connect to your Raspberry Pi to confirm the connection, e.g. `ssh pi@raspberrypi.local -- hostname`
1. Push to your bare repo: `git push origin`
	* Confirm in the `git push origin` output that the project builds and runs successfully on the Raspberry Pi
1. Open a web browser and go to [http://raspberrypi.local:8080](http://raspberrypi.local:8080)
	* Confirm output: "It works!"

# Next Steps

[John Woolsey](https://www.woolseyworkshop.com/author/jwoolsey/) trailblazed this topic with [Controlling A Raspberry Pi From A Web Browser With Vapor 3](https://www.woolseyworkshop.com/2018/12/21/controlling-a-raspberry-pi-from-a-web-browser-with-vapor-3). Check it out for a tutorial on next steps, like how to integrate an LED into the program.

# Appendix

## post-receive hook example

This post-receive hook:
1. changes the active directory to the working git repo,
2. fetches new changes from the bare repo and resets its state to match,
3. executes the Procfile script and logs its output to a file in /tmp.

```bash
$ chmod +x /git/VaporberryPi.git/hooks/post-receive

$ ls -ld /git/VaporberryPi.git/hooks/post-receive
-rwxr-xr-x 1 pi pi 254 Mar 10 14:11 /git/VaporberryPi.git/hooks/post-receive

$ cat /git/VaporberryPi.git/hooks/post-receive
#!/bin/bash -e
code_dir=~/VaporberryPi
cd "$code_dir"
git --git-dir "$code_dir/.git" --work-tree "$code_dir" fetch --all origin master
git --git-dir "$code_dir/.git" --work-tree "$code_dir" reset --hard origin/master
git --git-dir "$code_dir/.git" --work-tree "$code_dir" clean --force
log=/tmp/"$(basename "$code_dir")"-post-receive.log
./Procfile | tee "$log"
exit
```

## Vapor
<p align="center">
    <img src="https://user-images.githubusercontent.com/1342803/36623515-7293b4ec-18d3-11e8-85ab-4e2f8fb38fbd.png" width="320" alt="API Template">
    <br>
    <br>
    <a href="http://docs.vapor.codes/3.0/">
        <img src="http://img.shields.io/badge/read_the-docs-2196f3.svg" alt="Documentation">
    </a>
    <a href="https://discord.gg/vapor">
        <img src="https://img.shields.io/discord/431917998102675485.svg" alt="Team Chat">
    </a>
    <a href="LICENSE">
        <img src="http://img.shields.io/badge/license-MIT-brightgreen.svg" alt="MIT License">
    </a>
    <a href="https://circleci.com/gh/vapor/api-template">
        <img src="https://circleci.com/gh/vapor/api-template.svg?style=shield" alt="Continuous Integration">
    </a>
    <a href="https://swift.org">
        <img src="http://img.shields.io/badge/swift-4.1-brightgreen.svg" alt="Swift 4.1">
    </a>
</p>

