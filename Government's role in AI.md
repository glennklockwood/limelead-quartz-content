---
title: Government's role in AI
---
This is a messy collection of notes I've been maintaining with an eventual goal of writing a blog post about this. I don't know if that will ever happen.

## Infrastructure

The [[DOE]] Secretary of Energy Advisory Board (SEAB) created a report on [Recommendations on Powering Artificial Intelligence and Data Center Infrastructure](https://www.energy.gov/sites/default/files/2024-08/Powering%20AI%20and%20Data%20Center%20Infrastructure%20Recommendations%20July%202024.pdf).

## Opinions of others

Matt Hourihan posted a call to action which struck a nerve with me: [Why America Must Invest in DOE Labs To Win the AI Race Against China | RealClearDefense](https://www.realcleardefense.com/articles/2024/03/14/why_america_must_invest_in_doe_labs_to_win_the_ai_race_against_china_1018211.html). I personally don’t care about the CCP stuff (yet); more power to them from the standpoint of innovation. But my perspective is admittedly myopic in this area.

Bruce Schneier called for government-created foundation models ([Essays: Public AI as an Alternative to Corporate AI - Schneier on Security](https://www.schneier.com/essays/archives/2024/03/public-ai-as-an-alternative-to-corporate-ai.html)). Ideally sure, but practical hurdles make this untenable.

## Government's flakiness

Government is flaky and volatile; budget swings of 8%[^nsf-budget] reflect a lack of long-term commitment, and this uncertainty and lack of conviction is deeply harmful for public-private partnership and talent retention.

From [The Returns of Government R&D: Evidence from U.S. Appropriations Shocks](https://andrewjfieldhouse.com/wp-content/uploads/2023/12/The_Return_to_Government_R_D_manuscript.pdf):


> [!quote] 
> Based on a narrative classification of all significant postwar changes in R&D appropriations for five major federal agencies, we find that an increase in nondefense R&D appropriations leads to increases in various measures of innovative activity and higher business-sector productivity in the long run. \[...\] The estimates indicate that government-funded R&D accounts for one quarter of business-sector TFP growth since WWII, and imply substantial underfunding of nondefense R&D.

[^nsf-budget]: [Analysis: How NSF’s budget got hammered | Science | AAAS](https://www.science.org/content/article/analysis-how-nsf-s-budget-got-hammered).

## Government is ice skating uphill

Industry leads innovation in AI research, infrastructure, and people gravity. Consider [[Eagle]] (Microsoft) and [[Meta's H100 clusters]] (Meta). Also see anecdotes in [[LLM training at scale]].

50 MW sounds big to the government, but it's not for industry. Industry isn’t constrained like government is by having to build data centers based on politics rather than space, power, and cooling. For example, see [How a small city in Iowa became an epicenter for advancing AI - Source (microsoft.com)](https://news.microsoft.com/source/features/ai/west-des-moines-iowa-ai-supercomputer/). A richer discussion is in [[sustainability in HPC]].

Government will never catch up because paying customers are more demanding than public researchers:

- Who cares if [[Frontier]] is late and you can’t run HACC at a new breakthrough scale for another six to twelve months?
- By comparison, fortunes being made on lead times measured in months.
- This may settle out, but the AI industry will be pushed harder than government.

## What is the role of government then?

The ship has sailed on the government playing a significant role in AI leadership, but traditional HPC for scientific computing is still fair game because it has unique requirements:

- High precision arithmetic (FP64)
- Fortran

There are also elements of AI in which the government has unique interest:

- Trustworthiness, safety, and security
- AI for science

## How can government do better?

How can the government move forward?

1. **Give up**: Build staffing plans that assume constant turnover like the postdoc programs associated with [[DOE]]'s big HPC procurements (NESAP, CAAR, and ESP) as people graduate from government to industry.
2. **Lean *hard* into the right incentives**: Sabbaticals, joint appointments, and other ways to raise the ceiling. Work-life balance is a bad motivator; highly motivated people are going to work way too much regardless of where they work, and many big tech companies still offer work-life balance.

[Mobilizing Tech Talent: Hiring Technologists to Power Better Government](https://ourpublicservice.org/wp-content/uploads/2018/09/Mobilizing_Tech_Talent-2018.09.26.pdf) has a “Seven Strategies to Strengthen the Federal Government’s Technical Workforce” which is quite good at its surface:

1. Hire, appoint, and empower leaders with knowledge of modern technology
2. Use private-sector best practices to recruit and hire tech talent
3. Create the conditions for success
4. Upgrade the technical skills and competencies of the existing workforce
5. Build the brand and tell more stories
6. Remove structural barriers and make operational excellence possible
7. Consider ideas for future exploration

All of these are applicable to any team, not just public sector, though the implementation of each may have unique aspects to government. Interestingly, this paper does not discuss pay at all.

## My opinion

I am bearish of the government's ability to add value to the LLM game and wrote about it in my [ISC'24 recap blog post](https://blog.glennklockwood.com/2024/05/isc24-recap.html#section3).

I read a lot of these calls to action and strategic initiatives, and I’ve responded to many on my blog:

1. [ISC’24 recap (glennklockwood.com)](https://blog.glennklockwood.com/2024/05/isc24-recap.html) has my most direct, point-by-point refutation of many of the key arguments being made
2. [Life and leaving NERSC (glennklockwood.com)](https://blog.glennklockwood.com/2022/05/life-and-leaving-nersc.html) has an older, pre-joining-Microsoft perspective on where I saw the future of the Labs going
3. [Thoughts on the NSF Future Directions Interim Report (glennklockwood.com)](https://blog.glennklockwood.com/2015/01/thoughts-on-nsf-future-directions.html) speaks to the bad trajectory of NSF's HPC efforts as of 2015.