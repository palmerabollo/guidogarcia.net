---
author: admin
date: 2013-02-17 20:23:26+00:00
draft: false
title: Deploy virtual machines on Instant Servers cloud with Java
type: post
url: /blog/2013/02/17/deploy-virtual-machines-on-instant-servers-cloud-with-java/
categories:
- Software
- Tecnología
tags:
- api
- cloud
- iaas
- instantservers
- java
- virtualization
---

[Instant Servers](http://cloud.telefonica.com/instantservers/) is the infrastructure as a service (IaaS) system I have been working on during the last months in Telefónica Digital.

The service offers a public REST API (Cloud API) that is super simple to use. However, in this post I will show you how to manage your infrastructure using a Java client, without dealing with HTTP requests.

### Build the Cloud API client

Man does not live by nodejs alone. There is an [instantservers project at github](https://github.com/telefonicaid/instantservers.git) you can easily clone and compile (pull requests are also welcome). In the future it will be published as a proper maven artifact, so you can skip this point.

    git clone https://github.com/telefonicaid/instantservers.git
    cd ./instantservers/instantservers-api-client
    mvn install

That will generate an instantservers-api-client-1.0.0.M1.jar library you can use in your own applications.

### Deploy your first virtual machine

To deploy a virtual machine on Instant Servers cloud you only need to choose a **name** for the machine, a **package** that corresponds to the hardware configuration (cpu, mem, disk) you need, and a **dataset** that represents the image or template you want to use (i.e. ubuntu 12.04, mongodb, smartos, etc).

Let's code speak.

{{< highlight java >}}
package net.guidogarcia;

import com.tdigital.instantservers.model.cloud.Machine;

public class InstantServersExample {
    // there are several datacenters, I use Madrid "eu-mad" in this example
    private static final String CLOUDAPI_URL =
            "https://api-eu-mad-1.instantservers.telefonica.com";

    public static void main(String[] args) throws Exception {
        CloudAPIClient client =
                new CloudAPIClient("username", "password", CLOUDAPI_URL);

        Machine machine = new Machine();
        machine.setName("smallmachine");
        machine.setPackage("g1_standard_1cpu_512mb");
        machine.setDataset("sdc:sdc:smartos64:1.6.3");

        Machine deployed = client.createMachine(machine);
        System.out.printf("Machine id is %s", deployed.getId());
    }
}
{{< / highlight >}}

You will notice that virtual machines are up and running in a matter of seconds. This is due to the fact that the virtualization is based on rock solid [Solaris zones](http://en.wikipedia.org/wiki/Solaris_Containers).

You will need a username and a password to authenticate API calls, but you can [sign up for Instant Servers](http://cloud.telefonica.com/instantservers/) for free (machines are still not free but you can try it for something like 6 cents per hour).

If anyone is interested in other API operations or about cloud computing in general, leave a comment and I will be happy to write more posts about it.
