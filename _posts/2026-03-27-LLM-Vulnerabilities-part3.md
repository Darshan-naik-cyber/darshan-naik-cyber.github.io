---
title: LLM Hacking - Part 3 - Supply Chain | Data & Model Poisoning | Improper Output Handling
tags: [LLM Hacking , AI Security]
---

AI is getting stronger we all are known this condition, as far as many companies are realign with the base models from so that they can use that old good AI model & with the proper good fine tuning that model will work for them. Here what are we doing the we are using the old model means the pretrained model to build some product for the further use to the people. But by depending on this old model we are becoming victim of this crazy attack i.e., Supply Chain Attack.

### Supply Chain Attack


`I think this example is enough for you to understand how this supply chain attacks is work…if not then let me dig down into this particular article more.`

### Vulnerable Model (Pretrained Model)

Sometime we will take the low effort model those will give us the result in the instant way to make the user very good interactive so the user is creating that particular document for the base model & that will become threat to the entire model. Whyy??? cause at the time of the deployment they haven’t focused on the other factor of that pretrained model, they were just concern about there SEO.. to get the result in the better way… but in case of any chance if you model is not working properly then man defintly there is a huge risk that your model will suffer from the big supply chain… and you are using this model for the sensitive project. means man your MCP server as well as your complete product will be booooommmmm….. blast.

### Out of data or Vulnerable s/w , libraries or repositories

Here you have the good example i.e., of the Equifax , where company forgets to update Apache Server and that leads to loose the customer data in the very large way… means here they have neglected some basic compliance factor i.e., Patching. So if you are also using something old libraries or relying on some dependency or Repo then my sweet friend you data & application is at risk with this sort of vulnerability.

### Plugin Exploits

APIs Calling & Integration Stuff, Agents are mostly run with this sort of things where you have done the integration of the AI tool with some sort of Plugins & API. Now let us consider that , the plugins those you are using already into you AI agent. Already that plugin is outdated while still you are doing the integration of that plugin into your application. defintelyu your integration with that AI agent is not gonna work my man, cause at the end you will observe that AI agent is relying on that plugin is now going to be down by those vulnerabilities. so here the best recommendation is to get only those plugin those are having the less vulnerabilities or not having any associated vulnerability with it.

### Poisoned Package

This is one of the worst thing will happen if you found this vulnerability , cause something this might take a RCE means nothing but a creation of the backdoor to AI or LLM model. Ex. The PondRAT (Lighter version of POOLRAT) was the Remote Access Trojan, this RAT uploaded into the pyPI package by an attacker pyPI is a popular repository of open-source packages.so this benefit were taken by attacker , they embedded the Malware and boooom… many user’s had affected by using this poisoned package.


> There is famous example of the CVE-2024–8309 , This Vulnerability was discovered in the GraphCypherQAChain class of langchain-ai / langchain version 0.2.5 that allow for SQL injection through prompt injection. The vulnerability was disclosed on October 29, 2024 and this has been affects the langchain package version 0.2.5.

> So here this vulnerability allow attackers to perform SQL injection attacks through prompt injection. The issue is reached to the critical issue means at the higher severity level with network access vector.


`One More interesting thing regarding this vulnerability , One Australian guy uploaded a vulnerable code in one company and he got the access of the admin panel to manage and manipulate the data. This was the initial attacker vector who raised this crazy CVE.`

---

LLM Supply chain risk integrity due to vulnerability leading in biases, security breaches or system failure. Issue arise from pre-trained model, crowd source data & plugin extensions. so we are now clear with the above explanation what are those things.

---


