---
title: 'From DevOps to DataOps: A Fireside Chat on Practical Strategies for Effective Data Productivity'
date: 2024-08-27
slug: from-devops-to-dataops-effective-data-productivity
description: >
  dataops.... 
categories:
  - Announcements
  - Webinar
---
# From DevOps to DataOps: A Fireside Chat on Practical Strategies for Effective Data Productivity

Top priorities for data-driven organizations are data productivity, cost reduction, and error prevention. The four strategies to improve DataOps are: 

1. start with small, manageable improvements, 
2. follow a clear blueprint,
3. conduct regular data reviews, and
4. gradually introduce best practices across the team.

In a recent [fireside chat](../../firesidechat.md), [CL Kao](https://www.linkedin.com/in/clkao/), founder of [Recce](https://datarecce.io/), and [Noel Gomez](https://www.linkedin.com/in/noelgomez/), co-founder of [Datacoves](https://datacoves.com/), shared their combined experience of over two decades in the data and software industry. They discussed practical strategies to tackle these challenges, the evolution from DevOps to DataOps, and the need for companies to focus on data quality to avoid costly mistakes.

<figure markdown="span">
  ![Firesidechat banner](../assets/images/firesidechat-20240827/firesidechat_banner.jpg)
  <figcaption>Data Productivity - Beyonig DevOps &amp; dbt</figcaption>
</figure>

<!-- more -->

Noel Gomez began his journey at Amgen, where he worked extensively on data governance and digital transformation. He learned that data quality is essential to preventing errors. With his software development background, he realized that many software practices weren’t applied to analytics—and saw the opportunity to merge these worlds.

CL Kao worked on version control systems predating Git. He was involved more heavily in data when he helped start Taiwan’s civic tech community, focusing on making public data understandable.

Despite their different paths, both CL and Noel agree that many software practices can be adapted to data management, but there are critical differences. What are the similarities, and what sets DataOps apart?

### Similarities Between DevOps and DataOps

DevOps and DataOps stem from a shared philosophy:

1. DevOps and DataOps both emphasize **agility**, adapting quickly to changing requirements while maintaining efficiency, rooted in lean manufacturing and agile software development.
2. **Cross-functional collaboration** is essential in both practices. By reducing handoffs between teams, they ensure workflows are smooth. This helps keep pipelines running efficiently and deliver faster insights.
3. A **focus on quality** is key in both DevOps and DataOps. DevOps ensures clean, bug-free code and DataOps ensures accurate, reliable data for trustworthy decision-making insights.
4. **Infrastructure as code** is a foundational principle in both practices. It enables teams to manage systems through version-controlled code to ensure consistency across environments and reduce risks from manual configuration.
5. **Version control** in DevOps and DataOps allows teams to track changes, review updates, and roll back to previous versions to prevent errors in production.
6. **Testing and automation** are crucial in both practices. They help identify issues early and ensure reliability, with DevOps focusing on code quality and DataOps on data accuracy throughout the pipeline.

DevOps and DataOps share these principles, but applying them to data introduces unique challenges. While DevOps follows the well-established "preview, decide, deploy" cycle, DataOps often lacks this maturity. As CL and Noel pointed out, in many data environments, teams "deploy first, then decide"—leading to reactive fixes instead of proactive prevention.

Why is DataOps so different from DevOps, despite having the same underlying principles?

### Key Differences Between DevOps and DataOps

DevOps focuses on smooth software deployment without downtime, while DataOps ensures the data driving business decisions is accurate and trustworthy, while revisiting how we extract insights.

In DataOps, it’s not just about system uptime; the real challenge is preventing incorrect or incomplete data from leading to flawed decisions or costly errors. This introduces a new layer of complexity, as ensuring data accuracy can be more difficult than ensuring software stability.

A key distinction between DevOps and DataOps is the confidence teams have before releasing changes. In DevOps, you can deploy code changes confidently because everything has been tested in a controlled environment before production. In DataOps, even with the right tools, validating data is trickier because real-world data constantly changes and behaves unpredictably. It’s not just about running tests; it’s about verifying that the insights or automation from the data still make sense after transformations and processes.

### 1\. Different Goals

- **DevOps: Focus on Reliable Software Delivery** <br>
The primary goal of DevOps is to ensure software functions properly and consistently for users. Teams strive to deploy small, frequent changes confidently, without disruptions or compromising user experience. A significant part of this is validating the application performs as expected—whether it's a website or backend system—without crashes or performance degradation. <br>Even if DevOps achieves perfect functionality delivery, the data generated or consumed by the software may not be accurate. Noel pointed out, “Their website isn’t going down... but it’s still not delivering the data for analytics and decision-making.” This highlights a key limitation of DevOps: ensuring software stability doesn’t guarantee data correctness.

- **DataOps: Focus on Data Accuracy and Validity**<br>
DataOps focuses on validating data. The goal is to ensure systems are running and to confirm the data being used or processed is accurate and reliable. This means checking if the data makes sense—whether the joins, transformations, and calculations are correct and result in actionable insights. If data is incorrect or missing, even the best-maintained software won’t provide value, and businesses could make poor decisions based on faulty insights.

### 2\. The Validation Process

- **DevOps: Validating Applications**<br>
In DevOps, validation focuses on application functionality. Automated tests ensure the application runs correctly, features behave as expected, and the infrastructure remains stable. The primary concern is application stability while introducing changes—ensuring services are consistently available for users without unexpected crashes or interruptions.

- **DataOps: Validating Data**<br>
DataOps validation focuses on data correctness. Teams must ensure data transformations, aggregations, and insights are accurate before use in decision-making or customer-facing products. This involves automated tests and in-depth data checks to ensure changes in one part of the pipeline don’t affect other areas. For example, an innocent change could lead to missing or duplicated data, affecting downstream dashboards and reports. This makes DataOps more complex and crucial, as faulty data can lead to poor business decisions or financial losses, worsening with more automated systems.

### Why DataOps is More Complex
Ensuring data correctness introduces more complexity than ensuring software stability due to unique data management challenges, making DataOps a more intricate process.

1. **High Costs of Bad Data**<br>
Poor data quality can lead to significant financial losses, inefficiencies, and missed opportunities. Minor data errors can propagate through decision-making processes, leading to flawed insights and costly consequences. This makes the stakes in DataOps much higher than traditional DevOps.

2. **Data-Centric Software Stacks**<br>
As software systems become more data-driven, especially with AI and automated decision-making, managing data quality becomes complex. Without a coherent strategy, errors in data pipelines or transformations can lead to inaccurate decisions and a greater need for robust data management processes.

3. **Disconnected Teams and Legacy Processes**<br>
A common issue in DataOps is the disconnect between teams responsible for producing data and those using it, which leads to misunderstandings and inconsistencies. Many organizations still use outdated, manual processes with little awareness of modern data management practices. This absence of governance, code review, and version control makes it challenging to implement best practices and increases the risk of errors.

4. **Underrated Data Pipelines**<br>
Data pipelines are the backbone of decision-making processes, yet they often don't get the respect they deserve. While they may not be as flashy as AI models or analytics tools, everything depends on the quality and reliability of the data flowing through them. Inconsistent or poorly managed pipelines can lead to data issues, undermining the decision-making framework.

As the software stack becomes more data-centric and data plays a larger role in decision-making, especially in AI trends, this complexity increases. Without a structured approach to managing data, companies risk relying on flawed or inaccurate data, leading to costly mistakes.

<figure markdown="span">
  ![Firesidechat banner](../assets/images/firesidechat-20240827/bad-data-cost.png)
  <figcaption>bad data cost 15%~25% of Revenue</figcaption>
</figure>


### Practical DataOps Strategies

As the discussion dove into the root causes of DataOps challenges, CL and Noel shared four practical strategies from their work with various companies. Attendees were eager to ask: _How do you encourage teams using legacy systems and entrenched processes to adopt best practices? How do you recommend data professionals secure buy-in for implementing modern solutions?_

The four strategies to improve DataOps are: start with small, manageable improvements, follow a clear blueprint, conduct regular data reviews, and gradually introduce best practices across the team. Each step helps build a strong foundation for effective DataOps implementation.

If you're interested in their strategies and answers to these questions, as well as detailed advice on implementing DataOps in your organization,

**Sign up using the link below to access the full video recording**: [https://datarecce.io/firesidechat/](https://datarecce.io/firesidechat/)

### Closing Thoughts: A Brief Look at Datacoves and Recce

The fireside chat ended with something that stood out—the passion of these founders. After understanding the challenges teams face with data, CL and Noel focused on different key points, leading them to build two distinct but complementary products.

**Datacoves**, co-founded by Noel Gomez, focuses on accelerating DataOps workflows by integrating open-source tools like dbt. It's designed to be flexible, scalable, and provides teams a clear path to improve data quality without rigid systems. As Noel put it, "It's really about explaining to people and teaching them what's possible out there because we're very passionate about this kind of stuff."

**Recce**, founded by CL Kao, ensures data correctness across the pipeline. It simplifies validating data changes by curating proof of correctness, helping teams maintain confidence in their data as things get more complex. CL's vision is clear: "We are very passionate about the future of data-driven software and its development workflow."

What data problems resonate with you? Let's connect and chat!
