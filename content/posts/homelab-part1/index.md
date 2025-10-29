---
title: "Homelab Journey - Part 1: The Base"
date: 2025-10-29
draft: false
image: "cover.webp"
tags: ["homelab", "kubernetes", "cka", "ckad", "cks"]
categories: ["homelab", "Kubernetes"]
---

It finally happened — I decided to build my own **homelab**.  
For months, I had been telling myself, “You’ll need a real environment if you’re going to pass CKA, CKAD, and CKS.”  
And one quiet weekend, I finally gave in.

On my desk sat a modest Lenovo Ideapad, an HP ZBook ready for action, and a Jetson Nano waiting patiently on the shelf.  
Three machines, one mission: create a personal Kubernetes lab for hands-on practice.

#### The Plan

The idea was simple:

**Master Node:** Lenovo Ideapad  
- 6th Gen Intel CPU  
- 12 GB RAM  
- 256 GB SSD  

**Worker Node #1:** HP ZBook  
- 11th Gen Intel CPU  
- 16 GB RAM  
- 512 GB SSD  

**Worker Node #2:** Jetson Nano  
- ARM-based board  

This setup was not meant to be fancy or production-grade.  
Its only purpose was to prepare for **CKA**, **CKAD**, and **CKS** exams — a small, safe environment where I could break things, fix them, and truly understand how Kubernetes behaves under pressure.

#### The Master Node Story

Naturally, I started with the **master node** on the Lenovo Ideapad.  
I installed **Ubuntu Server** because it was the familiar choice. Everything went smoothly until I tried to connect to Wi-Fi.  
That was where things went wrong.

Ubuntu simply refused to cooperate.  
It didn’t detect the wireless card, didn’t see any networks, and refused to connect.  
I spent hours troubleshooting — drivers, firmware, kernel modules — the usual Linux cycle of hope and despair.

At some point, I accepted defeat.  
“Alright, Ubuntu. Not this time.”

#### Fedora to the Rescue

Next, I downloaded **Fedora Server** and installed it on the same machine.  
The process was clean, fast, and trouble-free.  
When the system booted, Wi-Fi was working immediately — no drivers, no manual tweaks, no issues.

That moment decided it: Fedora would be my **master node OS**.  
It worked instantly, and that reliability was exactly what I needed.

#### Kubeadm and Fedora Docs

For the Kubernetes setup, I followed Fedora’s official documentation:  
[Using Kubernetes (kubeadm) — Fedora Docs](https://docs.fedoraproject.org/en-US/quick-docs/using-kubernetes-kubeadm/)

The guide was well-structured and practical.  
I installed the required packages, initialized the cluster, configured access, and prepared the node for networking.  
When the setup completed successfully and I saw the cluster ready, it felt like the **homelab had officially come to life**.

#### The Next Crew: HP and Jetson

With the master node up and running, the next step was to add the worker nodes.  
The HP ZBook, with its newer CPU and higher memory, would be the first.  
The Jetson Nano would follow — a different architecture and a bit of a challenge, but a valuable learning experience.

This mixed setup would allow me to observe how Kubernetes handles **heterogeneous nodes**, something rarely encountered in cloud environments.

#### What’s Next

In **Part 2**, I will focus on:  
- Joining the HP ZBook to the cluster  
- Integrating the Jetson Nano  
- Setting up the network layer (likely Calico)  
- Running the first workloads  

If you are considering setting up your own homelab, my advice is simple: just start.  
You don’t need expensive servers or enterprise hardware.  
An old laptop and some curiosity are more than enough.

Building, breaking, and fixing your own cluster teaches more than any tutorial or course ever could.  
The new base is ready. The journey begins.
