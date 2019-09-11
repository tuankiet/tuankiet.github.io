---
layout: default
comments: true
title: "Adding swap space to an EC2 Amazon Linux instance"
date: 2017-11-02
categories: ruby
---

# Adding swap space to an EC2 Amazon Linux instance

## Instance specs:

- With instance EC2 T2.micro 1.0 GiB  1 vCPUs (http://www.ec2instances.info/?cost_duration=monthly&selected=t2.micro)

## Steps:

- Here are the steps I took to enable swap space:


`dd if=/dev/zero of=/swapfile bs=1M count=512`

`mkswap /swapfile`

`chmod 600 /swapfile`

`swapon /swapfile`


- You also have to edit your fstab file so the swap is available after reboot:

`sudo vim /etc/fstab`

- Add the following to the bottom of the file:

`/swapfile swap swap defaults 0 0`