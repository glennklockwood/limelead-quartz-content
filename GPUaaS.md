GPUaaS is a class of companies that offer pure-play GPU server access through a cloud-like consumption model. They typically follow a pattern whereby:

- They started as bitcoin mining companies during the rush and invested in building/buying data centers with high power densities
- When bitcoin collapsed, NVIDIA "invested" in them by giving them the ability to buy large quantities of NVIDIA GPUs at low cost
- These providers stick these GPUs in whitebox servers (Supermicro, Dell, etc), use open-source cloud software stacks on top, and sell access as a barebones cloud service.
- They are not technically opinionated, so they will deploy GPU clusters and accompanying storage in whatever architecture a customer wants

Cynically, NVIDIA uses these companies to:

1. get GPU hardware off its books, so NVIDIA's sales volumes look great
2. get someone else to deal with the burden of operating servers, data centers, and other infrastructure which is an open-ended cost

AI startups use these companies to get access to very large GPU clusters without needing to have all the money up front. If they go bankrupt before the GPUs are fully depreciated, these GPUaaS providers are stuck holding the depreciated assets (GPUs). NVIDIA gets paid, and AI companies get GPUs without a massive capital investment.

## Applied Digital

Applied Digital is a crypto-turned-GPUaaS provider based in Texas.

They appear to use Supermicro hardware[^ad-smci] and receive investment from NVIDIA.[^ad-nvda]

They partnered with [[Weka]] for storage. Weka advertised this deal,[^ad-weka] but strangely, Applied Digital did not. Unclear if this means the deal is not exclusive.

They are building DCs in North Dakota (see <https://aberdeeninsider.com/construction-continues-on-1-3b-data-center-in-ellendale/>)

[^ad-weka]: [WEKA Partners with Applied Digital to Supercharge Its GPU Cloud for Generative AI Customers](https://www.weka.io/company/weka-newsroom/press-releases/weka-partners-with-applied-digital-to-supercharge-its-gpu-cloud-for-generative-ai-customers/)
[^ad-smci]: [Applied Digital Teams with Supermicro to Deliver AI Cloud Services](https://ir.applieddigital.com/news-events/press-releases/detail/58/applied-digital-teams-with-supermicro-to-deliver-ai-cloud)
[^ad-nvda]: [Nvidia and Other Investors Back Applied Digital With $160 Million in Funding](https://www.wsj.com/tech/ai/nvidia-and-other-investors-back-applied-digital-with-160-million-in-funding-76157532?st=D5EPnW&reflink=article_copyURL_share)

## CoreWeave

CoreWeave is a crypto-turned-GPUaaS provider based in New Jersey. They seem to be leading this space and have received heavy investment from NVIDIA.

They partnered with [[VAST]] for storage.

## Lambda Labs

Lambda Labs has long been in the GPU business and started as a seller of GPU workstations.

They have an agreement with [[VAST]] for storage.

## Vultr

Vultr started as a traditional Virtual Private Server (VPS) provider rather than crypto, but they are now in the GPUaaS space as well.

Last I checked, they do not have an exclusive storage partner and will deploy whatever a customer wants them to.