---
title: "Kubeastronaut Adventure – Part 2"
date: 2025-10-05
draft: false
summary: "The second checkpoint of the Kubeastronaut journey — diving into security with the KCSA exam, limited resources, NotebookLM-powered notes, and personal reflections on the learning path."
image: "cover.webp"
tags: ["kcsa", "kubeastronaut", "kubernetes"]
categories: ["kubeastronaut", "kubernetes", "kcsa"]
readingTime: true
showToc: false
---

I’ve reached the second checkpoint of my Kubeastronaut journey!
This time, it’s all about security — a topic I honestly never cared much about. I used to think, “Come on, I just want to make the system work. Let someone else deal with security.” 😄  

Well, that’s half a joke, of course. In reality, you do need to make sure that what you build is secure. There’s no such thing as a system that *can’t* be hacked, but you can at least make sure the basics are covered. For example, don’t set the username to *admin* and the password to *admin*. At least write *admin* backwards or something. 😎

Preparing for the KCSA exam was quite an experience — mainly because, honestly, there aren’t many resources out there. Or maybe I just couldn’t find them. I managed to come across two different mock exams, which were incredibly helpful. I’ll share the links at the end of this post.

Since there weren’t many materials, I relied heavily on **NotebookLM** this time. It’s great for creating quizzes and flashcards. I uploaded my existing Kubernetes notes and asked it to generate questions and flashcards for me. That’s how I practiced most of the time.

Because of the limited resources, I thought, “Maybe I should take a look at CKS — after all, KCSA is basically its smaller sibling.” So I found a Udemy course about CKS. I didn’t finish it completely, but the instructor was really good, and it helped a lot.

As for the exam itself… I studied, but not in too much detail. It’s an associate-level certification — how hard could it be, right? And honestly, it wasn’t hard, but some questions came from topics I barely looked at, which was a bit scary. I remember thinking, “What’s NIST again?” I think I got two or three questions about that. I can’t remember exactly because of the exam stress, but that part was a little intimidating. The rest of the questions were fine — pretty familiar stuff, actually. Maybe I just knew a bit more than I thought.

In my daily work, I mostly use Kubernetes as a service from cloud providers, so they take care of a lot of the heavy lifting. That means I can sometimes skip over details that are already handled for me. It makes life easier, sure — but also a bit too comfortable. So I decided to build my own **homelab** this time. I was getting bored at home anyway, and it sounded like a fun challenge. I’ll write another post once I start setting it up — what I bought, how I built it, and what I learned from it.

**Useful links:**

- [Kubernetes Security KCSA Mock Exam (Thiago S. Shimada Ramos)](https://kubernetes-security-kcsa-mock.vercel.app/)
- [Udemy – Certified Kubernetes Security Specialist (CKS) Course](https://www.udemy.com/course/certified-kubernetes-security-specialist-certification/)
- *Certified Kubernetes Security Specialist (CKS) Study Guide* — Benjamin Muschko
- [NotebookLM](https://notebooklm.google)

So that’s another checkpoint cleared — **KCSA**, done. Next up are the big ones: **CKA** and **CKAD** in November, and **CKS** in December. Until then, I’ll keep studying — maybe by then, I’ll finally become a real astronaut.
