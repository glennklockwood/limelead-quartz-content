---
id: 9b401603-2bff-4323-8d86-8f76a1fb7c20
title: |
  Notice of Request for Information (RFI) on Frontiers in AI for Science, Security, and Technology (FASST) Initiative
author: |
  unknown
source: https://www.govinfo.gov/content/pkg/FR-2024-09-12/pdf/2024-20676.pdf
date_saved: 2024-09-26 13:10:22
draft: false
tags:
  - omnivore
---
These are some personal thoughts I've had in response to [Notice of Request for Information (RFI) on Frontiers in AI for Science, Security, and Technology (FASST) Initiative](https://www.govinfo.gov/content/pkg/FR-2024-09-12/pdf/2024-20676.pdf). In the spirit of [working with the garage door up](https://notes.andymatuschak.org/Work_with_the_garage_door_up), I'm working on these thoughts out in the open, and my intent is to publish an open response on [my blog](https://blog.glennklockwood.com/) (and maybe even formally submit a response as a concerned citizen) before the November 11 deadline.

> [!quote]
> This RFI seeks public input to inform  how DOE can partner with outside  institutions and leverage its assets to  implement and develop the roadmap for  FASST, based on the four pillars of  FASST: AI-ready data; Frontier-Scale AI  Computing Infrastructure and Platforms;  Safe, Secure, and Trustworthy AI  Models and Systems; and AI  Applications; as well as considerations  for workforce and FASST governance. [^9b401603-2bff-4323-8d86-8f76a1fb7c20]

## 2. Compute

> [!quote]
> (a) How can DOE ensure FASST  investments support a competitive  hardware ecosystem and maintain  American leadership in AI compute,  including through DOE’s existing AI  and high-performance-computing  testbeds?[^9b401603-2bff-4323-8d86-8f76a1fb7c20]

DOE must first define “American leadership in AI compute” very precisely. At present, American leadership in AI has happened in parallel to the US Exascale efforts; the race to achieve artificial general intelligence (and the AI innovation resulting from it) is being funded exclusively by private industry. The largest threats to American leadership in AI compute lie in factors that are secondary to the hardware ecosystem and include:

- Heavy-handed or misguided regulation that hampers the pace of innovation
- The domestic talent pool being constrained by uneven access to high-quality, affordable education that emphasizes STEM and critical thinking skills across the nation
- The global talent pool being inaccessible to American companies due to geopolitical tensions and immigration policy

It is not clear that FASST has the ability to address any of these threats directly, but FASST has the ability to elevate the level of discourse around AI within DOE and, by extension, enabling the federal government to meaningfully respond to these threats.

Directly supporting a competitive hardware ecosystem for AI compute will be a challenge for FASST.  Consider that NVIDIA, which holds an overwhelming majority of the market share of AI accelerators, recently disclosed in a 10-Q filing disclosed that almost half of its quarterly revenue came from four customers who purchased in volumes that exceed the purchasing power of ASCR and NNSA programs.[^nvda10q] It follows that the hardware ecosystem is largely shaped by the needs of a few key corporations, and the DOE no longer serves as a market maker with the purchasing power to sustain competition by itself. Thus, the DOE should consider partnering with other like-minded consumers of AI technology with similarly high risk tolerances to create a meaningful market for competition.

Collaborations like the now-defunct APEX and CORAL programs seemed like a step in this direction, and cross-agency efforts such as NAIRR also hold the potential for the government to send a unified signal to industry that there is a market for alternate technologies. If formally aligning FASST with parallel government efforts proves untenable, FASST should do all in its power to avoid contradicting those other efforts and causing destructive interference in the voice of the government.

[^nvda10q]: [FORM 10-Q](https://d18rn0p25nwr6d.cloudfront.net/CIK-0001045810/78501ce3-7816-4c4d-8688-53dd140df456.pdf); see also [Nearly half of Nvidia’s revenue comes from just four mystery whales each buying $3 billion–plus](https://fortune.com/2024/08/29/nvidia-jensen-huang-ai-customers/)

> [!quote]
> (b) How can DOE improve awareness of existing allocation processes for DOE’s AI-capable supercomputers and AI testbeds for smaller companies and newer research teams? How should DOE evaluate compute resource allocation strategies for large-scale foundation-model training and/or other AI use cases?[^9b401603-2bff-4323-8d86-8f76a1fb7c20]

The DOE's ERCAP model for allocations is already aligned with the way in which private sector matches AI compute consumers with AI compute providers. When an AI startup gets its first funding round, it is often accompanied with connections to one or more GPU service providers as part of the investment since such startups' success is contingent upon having access to reliable, high-performance computing capabilities.[^nyt][^m12] Continuing this model through FASST is the most direct way to raise awareness amongst those small businesses and researchers who stand to benefit most from FASST resources.

Evaluating allocation strategies should follow a different model, though. Recognizing that the centroid of AI expertise in the country lies outside of the government research space, FASST allocations should leverage AI experts outside of the government research space as well. This approach will have several benefits:

- It reduces the odds of allocated resources being squandered on research projects that, while novel to the scientific research community, may have known flaws to the AI community.
- It also keeps DOE-sponsored AI research grounded to the mainstream momentum of AI research, which occurs beyond the ken of federal sponsorship.

DOE should also make the process fast because AI is fast.

[^nyt]: [The Desperate Hunt for the A.I. Boom’s Most Indispensable Prize](https://www.nytimes.com/2023/08/16/technology/ai-gpu-chips-shortage.html)
[^m12]: [Startups to access high-performance Azure infrastructure, accelerating AI breakthroughs](https://startups.microsoft.com/blog/startups-to-access-high-performance-azure-infrastructure-accelerating-ai-breakthroughs/)

> [!quote]
> (d) How can DOE continue to support  the development of AI hardware,  algorithms, and platforms tailored for  science and engineering applications in  cases where the needs of those  applications differ from the needs of  commodity AI applications? How can  DOE partner with other compute  capability providers, including both on-  premises and cloud solution providers,  to support various hardware  technologies and provide a portfolio of  compute capabilities for its mission  areas?[^9b401603-2bff-4323-8d86-8f76a1fb7c20]

## 3. Models

> [!quote]
> (b) How can DOE support investment  and innovation in energy efficient AI  model architectures and deployment, including potentially through prize-based competitions? [^9b401603-2bff-4323-8d86-8f76a1fb7c20][^9b401603-2bff-4323-8d86-8f76a1fb7c20]

Energy efficiency is a red herring when the true concern is carbon emissions up to scope 3. Do not focus solely on energy efficiency, because that is a geographically constrained subset of the true problem.

## 4. Applications

> [!quote]
> (b) How can DOE ensure foundation  AI models are effectively developed to  realize breakthrough applications, in  partnership with industry, academia,  and other agencies? [^9b401603-2bff-4323-8d86-8f76a1fb7c20]

“Foundation models” are being used disingenuously to justify capex. Realize that foundation models for science are different than foundation models for language. Question whether a science-specific foundation model is truly worthwhile given practitioner comment at SMC and the BloombergGPT example.

## 5. Workforce

> [!quote]
> (a) DOE has an inventory of AI  workforce training programs underway  through our national labs.4 What other  partnerships or convenings could DOE  host or develop to support an AI ready  scientific workforce in the United  States?[^9b401603-2bff-4323-8d86-8f76a1fb7c20]

## 6. Governance

> [!quote]
> (a) How can DOE effectively engage  and partner with industry and civil  society? What are convenings,  organizational structures, and  engagement mechanisms that DOE  should consider for FASST?[^9b401603-2bff-4323-8d86-8f76a1fb7c20]

DOE needs to keep pace with the AI industry which does not limit itself with months-long FOAs preceding work. It also does not limit itself to judging progress based on traditional peer review in conferences and journals; consider how many foundational AI papers were only published on arxiv. 

> [!quote]
> (b) What role should public-private  partnerships play in FASST? What  problems or topics should be the focus  of these partnerships? [^9b401603-2bff-4323-8d86-8f76a1fb7c20]

FASST should establish vehicles for public-private partnerships that go beyond the conventional large-scale system procurements and large-scale NRE projects. Both of these vehicles reinforce a customer-supplier relationship where money is exchanged for goods and services, but the new reality is that the AI industry is least constrained by money. FASST must provide incentives that outweigh the opportunity cost that partners face when dedicating resources to DOE and its mission instead of the commercial AI industry, and the promise of low-margin large-scale capital acquisition contracts is simply not sufficient.

To put this in concrete terms, I am frequently faced with the dilemma: is my time better spent writing a response to a government RFI such as this one or developing insight that will improve the overall reliability of our next flagship training cluster? 

Realistically, investing my time in the government has a negligible role in "moving the needle" in a meaningful way. It may slightly increase the chances that DOE would award a major system procurement to me as an AI company, but even then, that business would have low gross margin due to the extensive requirements and oversight that accompanies those procurements. A similarly sized opportunity could surface with a twenty-person AI startup with higher margins backed by private equity and a more agile approach to partnership.

The latter may reduce the overall training time for the next frontier model by 5%, save millions of dollars in spend, and increase our competitive lead by weeks. The answer is always to choose the latter, resulting in the former being relegated to a passion project which consumes my nights and weekends. FASST should find ways to make this dilemma much less black and white.

## Response guidelines

> [!quote]
> Commenters are welcome to comment on any question. RFI responses shall include:
> 
> 1. RFI title;
> 2. Name(s), phone number(s), and email address(es) for the principal point(s) of contact;
> 3. Institution or organization affiliation and postal address; and
> 4. Clear indication of the specific question(s) to which you are responding.
> 
> Responses to this RFI must be submitted electronically to FASST@ hq.doe.gov with the subject line ‘‘FASST RFI’’ no later than 5:00 p.m. (ET) on November 11, 2024. Responses must be provided as attachments to an email. It is recommended that attachments with file sizes exceeding 25 MB be compressed (i.e., zipped) to ensure message delivery. Responses must be provided as a Microsoft Word (*.docx) or Adobe Acrobat (*.pdf) attachment to the email and should be no more than 15 pages in length, 12- point font, 1-inch margins. Only electronic responses will be accepted. Only one response per individual or organization will be accepted.
> 
> A response to this RFI will not be viewed as a binding commitment to develop or pursue the project or ideas discussed. DOE may engage in postresponse conversations with interested parties.

[^9b401603-2bff-4323-8d86-8f76a1fb7c20]: [Notice of Request for Information (RFI) on Frontiers in AI for Science, Security, and Technology (FASST) Initiative](https://www.govinfo.gov/content/pkg/FR-2024-09-12/pdf/2024-20676.pdf)