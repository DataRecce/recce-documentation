---
title: 'Support Self-Serve Data with Comprehensive PR Review'
date: 2024-09-16
slug: self-serve-analytics-pr-review
description: >
  dbt helps to apply software engineering (DevOps) best practices to data pipelines. With proper adoption and application of these practices as Dataops it is possible to support a self-serve data and analytics culture, while still maintaining data integrity through systematic data validation and PR review.
categories:
  - Concepts
  - Self-Serve Review
  - dbt
tags:
  - Data Validaton
  - Data Integrity
  - Self-Serve Data
  - Self-Serve Analytics
  - dbt CI
  - dbt PR Review
  - Data Review
  - Best Practices
  - Data Quality
  - DataOps
---
# The Guide to Supporting Self-Serve Data and Analytics with Comprehensive PR Review

dbt, the platform that popularized ELT, has revolutionized the way data teams create and maintain data pipelines. The key is in the ‘T’, of ELT. Rather than transforming data before it hits the data warehouse, as in traditional E*T*L, dbt flips this and promotes loading raw data into your data warehouse and transforming it there, thus EL*T*.

This, along with bringing analytics *inside* the data project, makes dbt an interesting solution for data teams looking to maintain a single-source-of-truth(SSoT) for their data, ensuring data quality and integrity.

<figure markdown="span">
  ![Move Fast and DON'T Break Prod](../assets/images/support-self-serve-data-20240916/move-fast.png)
  <figcaption>Move Fast and DON'T Break Prod</figcaption>
</figure>

<!-- more -->


**dbt transformations are stored as a series of SQL models** that transform the data through raw, staging, intermediate, and then mart stages, in which the data is ready for production use.  Keeping all of these transformations in the same place, as the SSoT for how your data is generated, ensures that data validation and data quality checks can be systematically applied at every stage. This also facilitates PR review to maintain data integrity.

- **Version control** - the dbt project can be managed in a version control system such as git, essential for dbt CI workflows and PR review.
- **Modular SQL** - dbt encourages a modular design to data pipelines. This better facilitates reviewing data models and transformed data, and enabling the re-use and referencing of data models.
- **Reproducible pipelines** - the dbt project contains everything required to run the pipeline, which means anyone can checkout the project from version control and build the data. This is a fundemental part of data best practices and dataops.

If you’ve come from a software engineering (SE) background, then these benefits may be familiar to you. Version control, modularity, and reproducibility are **the tenets of DevOps**, and have benefited software development for years. Through dbt, you’re able to adapt them as part of a new practice of **DataOps,** and **the pull request (PR) is at the center of the process.**

## Data Incidents still happen

Given the benefits that dbt brings, you’d be forgiven for thinking that the data problem was solved - That data was always accurate, and bad data was never merged into production. Unfortunately, due to the relative infancy of DataOps, that’s still not the case. Data incidents still happen.

As CL Kao, founder of [Data Recce](https://datarecce.io/) put it in a recent [fireside chat](https://datarecce.io/firesidechat/) with Noel Gomez of [Datacoves](https://datacoves.com/):

> <i>“The construct that turned devops from a best practice into a viable, productionized, practice, is the pull request… preview, decide, deploy. (However) **In data, we deploy, and then decide if we want to fix it later**”</i><br />- **CL Kao**, [Data Productivity - Beyond DevOps and dbt](https://datarecce.io/firesidechat/)
> 

The “deploy and then fix it later” quote probably rings true for a lot of data teams that are working under tight deadlines, with many simultaneous PRs, and find it difficult to perform due diligence on every PR before merging.

### Look ma, no guardrails!

The version-controlled nature of dbt makes code review straightforward, but data review remains a challenge. It’s vital to build guardrails around the PR review process to ensure that data quality is upheld and the data impact of changes is thoroughly assessed.

### How much does a data incident cost?

Merging bad data, whether that be from accidentally changing historic data, to introducing silent errors into production data, can be very costly. Data impact assessment prevents such incidents through rigorous data validation, data quality checks, and structured PR review - Avoiding substantial financial losses caused by bad data.

<figure markdown="span">
  ![The Cost of Data Incidents](../assets/images/support-self-serve-data-20240916/data-incidents.png)
  <figcaption>The Cost of Data Incidents</figcaption>
</figure>

Mikkel Dengsoe of Sync recently [calculated](https://medium.com/@mikldd/the-cost-of-data-incidents-53646b588601) that if the cost of a data incident ranges from $1,000 to $100,000 per-incident, that for a large data team that could result in costs of *“between $500,000 and $4,000,000 per year”*.

As Mikkel puts it, the value of data for business critical applications is high:

> *“A few hours of incorrect data for an eCommerce ad bidding machine learning model can easily cost $100,000.”*  Mikkel Dengsoe, [The cost of data incidents](https://medium.com/@mikldd/the-cost-of-data-incidents-53646b588601)
>


## Why dbt data PRs are hard to review

Firstly, data change is inherently hard to review. The pull request review process from software practices is not suited to reviewing data. PR review is all about reviewing code, not data.  When you open a pull request on a data project, **you can easily see how the SQL code** and transformation logic **has changed, but the data is still a black box**, and makes data impact assessment and data validation a unique challenge.

<figure markdown="span">
  ![New Challenges to Data Integrity](../assets/images/support-self-serve-data-20240916/new-challenges.png)
  <figcaption>New Challenges to Data Integrity</figcaption>
</figure>

Secondly, **dbt has introduced ‘self-serve data’.**, where more users within an organization can access and manipulate data. This shift means that dataops practices much evolve to ensure that data quality and data integrity is maintained as the number of data users grows.



<div style="border-radius: 1rem;  padding: 0.5rem 0 0.5rem 0; background-color: #efefef; text-align: center;">

  <img src="../assets/images/support-self-serve-data-20240916/pdf-download-recce-deck.png" class="skip-glightbox" />

  <div id="mc_embed_shell">
        <link href="//cdn-images.mailchimp.com/embedcode/classic-061523.css" rel="stylesheet" type="text/css">
    <style type="text/css">
          #mc_embed_signup{clear:left; font:14px Helvetica,Arial,sans-serif; width: 100%}
          /* Add your own Mailchimp form style overrides in your site stylesheet or in this style block.
             We recommend moving this block and the preceding CSS link to the HEAD of your HTML file. */
  </style>
  <div id="mc_embed_signup">
    <h2 style="text-align: center; color: #ff6e42; font-size: 1.2rem; margin-top: 0">Download the Deck</h2>

  <form action="https://datarecce.us1.list-manage.com/subscribe/post?u=2b18366427f11835f05f68aeb&amp;id=b82bfd845e&amp;f_id=00bd81e5f0" method="post" id="mc-embedded-subscribe-form" name="mc-embedded-subscribe-form" class="validate" target="_blank">
          <div id="mc_embed_signup_scroll" style="text-align: center;">
              <div class="indicates-required" style="display: none;"><span class="asterisk">*</span> indicates required</div>
              <div class="mc-field-group" style="width:100%;"><label for="mce-EMAIL" style="color: #000;">Enter your email address and we'll send you the presentation deck: <span class="asterisk">*</span></label><input type="email" name="EMAIL" class="required email" id="mce-EMAIL" required="" value="" style="background-color: #fff; color: #000; font-size: 0.9rem"></div>
  <div hidden=""><input type="hidden" name="tags" value="6327490"></div>
          <div id="mce-responses" class="clear">
              <div class="response" id="mce-error-response" style="display: none;"></div>
              <div class="response" id="mce-success-response" style="display: none;"></div>
          </div><div style="position: absolute; left: -5000px;" aria-hidden="true"><input type="text" name="b_2b18366427f11835f05f68aeb_b82bfd845e" tabindex="-1" value=""></div><div class="clear"><input type="submit" name="subscribe" id="mc-embedded-subscribe" class="button" value="Send me the PDF" style="background-color: #ff6e42; text-transform: uppercase; font-weight: bold; font-size: 0.9rem; padding: 0.3rem 1rem; height: auto;"></div>
      </div>
  </form>
  </div>
  <script type="text/javascript" src="//s3.amazonaws.com/downloads.mailchimp.com/js/mc-validate.js"></script><script type="text/javascript">(function($) {window.fnames = new Array(); window.ftypes = new Array();fnames[0]='EMAIL';ftypes[0]='email';fnames[1]='FNAME';ftypes[1]='text';fnames[2]='LNAME';ftypes[2]='text';fnames[3]='ROLE';ftypes[3]='text';}(jQuery));var $mcj = jQuery.noConflict(true);</script></div>

</div>

## What is self-serve data?

The core principle of self-serve data is to enable wider access to data without the need for specialized knowledge, or the help of data specialists. As [atlan](https://atlan.com/what-is/self-service-data/#what-is-self-service-data-what-are-its-key-characteristics) says, users should be able to “**access, analyze, and manipulate the data”** by themselves:

> Self-serve data refers to **tools… that enable …users** **to** access, analyze, and **manipulate data** **without** requiring the direct intervention of **data specialists**
— [atlan](https://atlan.com/what-is/self-service-data/#what-is-self-service-data-what-are-its-key-characteristics)
> 

[dbt](https://www.getdbt.com/blog/key-components-of-data-mesh-self-serve-data-platform#what-is-a-self-serve-data-platform) takes it a step further, in that **users should not require specialized knowledge** to create new data products:

> A self-serve data platform… supports **creating new data products** **without** the need for custom tooling or **specialized knowledge**.
— [dbt](https://www.getdbt.com/blog/key-components-of-data-mesh-self-serve-data-platform#what-is-a-self-serve-data-platform)
> 

All you need to know is SQL, and you can start using dbt and access and manipulate the data to your needs.

The actual implementation of self-serve data will differ from organization to organization, but at the very least you would expect that analysts will be shifting some analytics out of BI and directly into dbt. In other cases, non-technical (in engineering terms) teams, such as finance or marketing, may also be directly accessing the data project.

## Data, meet integrity

An initial worry from data engineering teams is that [self-serve data may introduce data integrity issues](https://medium.com/inthepipeline/what-comes-after-the-chaos-of-self-service-analytics-f0e09c0d7b40), and undermine the work of **data engineers who are traditionally tasked with maintaining the stability and integrity of data and data pipelines**. Indeed, to successfully adopt self-serve analytics, a shift in internal culture is required.

<figure markdown="span">
  ![Self-serve data doesn’t have to mean self-serve chaos](../assets/images/support-self-serve-data-20240916/data-chaos-in-4-acts.jpg)
  <figcaption>Self-serve data doesn’t have to mean self-serve chaos</figcaption>
</figure>

Rather than gatekeeping access to data, data engineering teams should actively support the self-serve nature of data. The **pull request process is central to maintaining data integrity.** It’s the stage in which **data quality is upheld, and the intention of the PR author defined and validated**.

## How to support a self-serve data culture

With the PR review being central to ensuring data integrity, the team must validate both the code and the data changes to understand the data impact. Following dbt best practices for data validation and data review is critical to building trust in self-serve data environments.

If the author of the PR records the steps that they took to validate the data generated from the models in the PR, this enables:

1. The PR author is able to **review their own work**, ensuring data quality.
2. The data team is able to **review the work of others** (peer review), enhancing DataOps processes.
3. The **data validation checks can be shared with others** for approval ensuring data integrity by enabling business-context review.

To enable this PR review workflow for data, the PR process for data projects needs to be augmented with suitable tools, enabling **'self-serve *review*' for self-serve data.**

<figure markdown="span">
  ![Data impact assessment is more important than ever](../assets/images/support-self-serve-data-20240916/finger-pointing.jpg)
  <figcaption>Data impact assessment is more important than ever</figcaption>
</figure>

### Communication overhead

As more people modify the data project, more PR review is required, bringing with it communication overhead. The team needs to communicate their adjustments and modifications to the data pipeline that require review, and the data team needs to be able to support this review.

To enable better comunication between data teams and stakeholders, we must first understand **what the review process for data actually looks like, and understand how to *review* data**.

## How to perform data impact assessment for PR review

Data impact is difficult to assess. You’re faced with the problem of having to interpret if the numbers you’re looking at are correct. How can you know that with confidence, ensuring data quality?

### 1. Show me the change

The best way to perform data impact assessment is to *see* exactly *how* the data has changed.

Benn Stancil, of Mode Analytics, takes a humorous look at this in a recent [blog post](https://benn.substack.com/p/all-i-want-is-to-know-whats-different). Benn provides two methods. The first, a complicated, convoluted process following the data through contract alerts, and log files. The second, simply:

> “Check if historical values of the metric are the same today as they were yesterday.” 
<br /> - **Benn Stancil**, [All I want to know is what’s different](https://benn.substack.com/p/all-i-want-is-to-know-whats-different)
> 

It’s that simple. You want to be able to easily **compare data from before-and-after making data model changes**. Doing this will, and confirming that metrics or key figures didn’t change will give you the **confidence to merge a PR knowing that unintended impact did not occur**, therefore maintaining data integrity.

<figure markdown="span">
  ![Just show me what's different](../assets/images/support-self-serve-data-20240916/show-me-whats-different.png)
  <figcaption>Just show me what's different</figcaption>
</figure>

### 2. Help me validate change

Confirming that data didn’t change is good for refactoring work and detecting unintended impact, but **there are also instances when you would *expect* to see data change** - You want to **confirm that the data is correct**, and that the SQL (data model logic change) has the  **intended data impact**.

Comparing data, and the structure of the data pipeline, before-and-after making changes will enable you to see data impact clearly. You want to be able to easily see newly added, removed, and modified data models, and perform data validation checks to ensure accuracy. By doing this, you can assess data quality and look into the models to see how that data has changed, ensuring data integrity throughout the PR review process.

### What’s your intention and expectation

To understand if data impact is correct, you need to already have an expectation of what to look for. This expectation is based on your intention, or, what you are trying to achieve. This is why the [pull request template](2024-09-25_dbt-data-pr-comment-template.md) is such a fundamental tool, it helps the PR author clarify these things.

- **Intention** - What you trying to do, such as fix a bug, add a new column, refactor a group of models etc.
- **Expectation** - Given you intention, what is the expected data impact once the work is completed? For example, no rows should change on models X and Y, or the average value of Z column should remain constant.

Once these are defined, the process of validating data impact becomes a lot easier. The main problem is then the mechanism to perform the data validation checks to prove your expectation, and the groundwork that is needed in your data project to enable that.

## Clarity through comparison

Verifying your work before pushing to production is the only way to be absolutely confident that there are no breaking changes. In order to do this, you need to have a duplication of your production environment, or a method to **reproduce, or replicate, a subset of production**, so that you have **a known-correct baseline to check your work against**.

Noel Gomez, of Datacoves, describes the importance of having **reproducible environments**:

> <i>“We want to make sure that we have reproducible environments… this allows you to experiment. …**you’re working in a different environment, you’re not going to break something**. (then) there should be a review before (it goes to production). **The goal is ‘Does this data make sense?’”**</i> <br /> - **Noel Gomez**, [Data Productivity - Beyond DevOps and dbt](https://datarecce.io/firesidechat/)
> 

### Reproducible environments in dbt

Following [dbt best practices](https://datarecce.io/docs/guides/best-practices-prep-env/) for how to replicate data environments include:

- **Per-developer** schemas - this allows the person modifying the pipeline to check their work by building models into their own schema. Enabling the developer (data engineer, analyst etc) to perform data quality checks before opening a PR.
- **Per-PR** schemas - Each time a PR is opened, the new project code should build the project into a PR-specific schema. This enables automated, dbt CI-time (continuous integration) checks to run (more on that below), helping with data validation and ensuring the PR review process is efficient.
- **Staging** schema (subset of prod) - As part of CD (continuous delivery) build a subset of production data into a separate schema. This acts as the known-good baseline and can be used to check developer and PR schemas against. The benefit is that you don’t need to replicate the whole of production, which can be time consuming and expensive.

As Noel mentions in the quote above, having the ability to replicate environments empowers developers—those modifying the data pipeline and validating changes—to confidently assess data quality and data impact without compromising production data. A strategy that streamlines the development process and also adheres to data integrity principles within a self-serve data culture.

## Data validation with Recce

Once you have a suitable system in place to manage your database environments during development and deployment, you’re then able to perform comparisons between these environments. This is where Recce can help enhance your data validation process and improve your dbt PR review.

**Recce provides a suite of tools that are specially designed to assess data impact** by comparing the data in two dbt environments.

- You’re clear what your **intention** is
- You’re clear what your **expectation** is
- Now, **prove it**

Recce helps you to prove that your data model changes have the intended impact through its data impact check framework. Recce data checks, and enable you to:

- **compare** your data
- **record** your findings
- **share** those findings

## Recce Checks

Recce checks are **individual data validations** that show the results of a specific type of comparison between your data environments. These checks help to prove that your intention was successful, and your expectation has been realized in the data. You use these check results for:

- **validating your work**, *as* you work
- sharing as **part of discussion** between colleagues or stakeholders
- sharing as **proof-of-correctness** of your work
- allowing others to **approve the results** for data validation

There are various check types that range from high level profiling, right down to row level data inspection. **Each type of check provides an insight into how data differs, or not, compared to the previous state.** By recording the results of your checks, you can demonstrate that proper impact assessment has been carried out and share the results with others for data QA. 

### Structural Checks

Structural checks provide an overview of structural impact to the data project.

- **Lineage Diff** - Shows how the lineage has changed in the PR and the dependencies between models.
- **Row Count Diff **- Shows the row count change for models that gives a high level indication that the amount of data is the same.
- **Schema Diff** - Shows if any models have added, removed, or renamed columns, providing insight into data structure modifications.

### Statistical Checks

Statistical checks provide a more detailed look at the data which helps to determine the type of data impact that may have occurred.

- **Profile Diff** - Shows the comparison of statistical profiles, such as mean, median, standard deviation, and distinct count, between the current and previous data.
- **Value Diff** - Shows the percentage matched rate for each column between the current and previous data.
- **Top-K Diff** - Shows the most frequent values in categorical fields between current and previous data.
- **Histogram Diff** (distribution) - Shows an overlay comparison of data distribution between current and previous data.

<figure markdown="span">
  ![Data Propfile Diff](../assets/images/support-self-serve-data-20240916/data-profile.jpg)
  <figcaption>Detect statistical anomalies with Data Profile Diff </figcaption>
</figure>

### Row-Level Checks

Row-level checks enable you to do data spot-checks on specific rows.

- **Query Diff** - Run any query and see how the results of that query differ between current and previous data.

Checks can be added to the Recce Checklist as part of your validation work, or they can be added automatically (see below).

## Recce checklist

Organized into a checklist, Recce checks help to collectively validate a PR, ensuring data quality and data integrity throughout the development process. Each check contains:

- **Check results** - The results of a specific check that helps to validate the success of the PR.
- **Author’s annotation** - A description that explains what is being shown and why the check results are vital for understanding the data quality and data integrity of the PR.

The checklist helps to guide the reviewer through the process that was used to validate the PR. When accompanied by a detailed PR comment, will enable the reviewer to understand the author's **intention** and **expectation**, allowing for informed sign-off on the PR or requests for further data checks.


## Automate checks for critical models (preset checks)

In addition to checks that fall within the scope of the PR, there should also be global checks that run regardless of the work being done for the PR. These global checks are called **Preset Checks** in Recce, are designed to ensure complete data quality coverage of your project, and target models that have:

- Significance importance to the business,
- Numerous downstream dependencies,
- Historical issues that may prompt inquiries about data integrity if problems arise (in other words, you'll get a call about that data if something goes wrong!)

Read more about the importance of [identifying and checking critical dbt models](https://datarecce.io/blog/check-critical-models/) as part of data validation strategry for dbt and maintaining data quality in dbt PR reviews.

## Share validation check results

Once you’ve curated a checklist of data validation checks, it’s time to share those checks in your PR comment. With Recce you can do this in two ways:

1. Share individual checks for focused data impact discussions
2. Share the Recce File, which contains the complete checklist and allows other team members to continue review or help validate impact.
3. Use Recce Cloud for to sync checks across Recce instances and block PR merging until all data impact has been assessed

### Sharing individual checks

Sharing individual checks can be useful for smaller PRs, or when you want to have discussion around the results. For more complex PRs, with many checks, the Recce File is the more powerful choice for collaborating on data validation.

### Sharing the Recce File

Exporting the Recce File offers two key advantages:

  1.  The full checklist is made available for review, enhancing transparency in your data validation processes.
  2.  Reviewers can interact with data checks and perform and save additional checks if necessary. 

The PR author can export the Recce File, then attach it to the PR, or save it in your organization’s shared storage platform. Another colleague can then run Recce in ‘review mode’ to start the PR review, facilitating thorough data validation and promoting teamwork in reviewing data PRs. 

### Recce Cloud

For those using Recce Cloud, the process is a lot smoother, with checks synced between Recce Instances, and the ability to launch a Recce Instance and review a PR within minutes from the Recce Cloud interface.

With Recce Cloud, there’s no need to export and share the Recce File, or deal with manually exporting checks.

Recce Cloud integrates with your GitHub PR and can block merging until all checks have been reviewed. 

<div style="border-radius: 1rem;  padding: 0.5rem 0 0.5rem 0; background-color: #efefef; text-align: center;">

  <img src="../assets/images/support-self-serve-data-20240916/pdf-download-recce-deck.png" class="skip-glightbox" />

  <div id="mc_embed_shell">
        <link href="//cdn-images.mailchimp.com/embedcode/classic-061523.css" rel="stylesheet" type="text/css">
    <style type="text/css">
          #mc_embed_signup{clear:left; font:14px Helvetica,Arial,sans-serif; width: 100%}
          /* Add your own Mailchimp form style overrides in your site stylesheet or in this style block.
             We recommend moving this block and the preceding CSS link to the HEAD of your HTML file. */
  </style>
  <div id="mc_embed_signup">
    <h2 style="text-align: center; color: #ff6e42; font-size: 1.2rem; margin-top: 0">Download the Deck</h2>

  <form action="https://datarecce.us1.list-manage.com/subscribe/post?u=2b18366427f11835f05f68aeb&amp;id=b82bfd845e&amp;f_id=00bd81e5f0" method="post" id="mc-embedded-subscribe-form" name="mc-embedded-subscribe-form" class="validate" target="_blank">
          <div id="mc_embed_signup_scroll" style="text-align: center;">
              <div class="indicates-required" style="display: none;"><span class="asterisk">*</span> indicates required</div>
              <div class="mc-field-group" style="width:100%;"><label for="mce-EMAIL" style="color: #000;">Enter your email address and we'll send you the presentation deck: <span class="asterisk">*</span></label><input type="email" name="EMAIL" class="required email" id="mce-EMAIL" required="" value="" style="background-color: #fff; color: #000; font-size: 0.9rem"></div>
  <div hidden=""><input type="hidden" name="tags" value="6327490"></div>
          <div id="mce-responses" class="clear">
              <div class="response" id="mce-error-response" style="display: none;"></div>
              <div class="response" id="mce-success-response" style="display: none;"></div>
          </div><div style="position: absolute; left: -5000px;" aria-hidden="true"><input type="text" name="b_2b18366427f11835f05f68aeb_b82bfd845e" tabindex="-1" value=""></div><div class="clear"><input type="submit" name="subscribe" id="mc-embedded-subscribe" class="button" value="Send me the PDF" style="background-color: #ff6e42; text-transform: uppercase; font-weight: bold; font-size: 0.9rem; padding: 0.3rem 1rem; height: auto;"></div>
      </div>
  </form>
  </div>
  <script type="text/javascript" src="//s3.amazonaws.com/downloads.mailchimp.com/js/mc-validate.js"></script><script type="text/javascript">(function($) {window.fnames = new Array(); window.ftypes = new Array();fnames[0]='EMAIL';ftypes[0]='email';fnames[1]='FNAME';ftypes[1]='text';fnames[2]='LNAME';ftypes[2]='text';fnames[3]='ROLE';ftypes[3]='text';}(jQuery));var $mcj = jQuery.noConflict(true);</script></div>

</div>

## Conclusion

dbt brings a lot of benefits to data projects by incorporating essential software engineering practices into the data analytics workflow. By embracing and adopting organizations can support a self-serve data culture that empowers teams to access and leverage data effectively, while also maintaining data integrity. 

For discussion on how to properly adopt devops best practices for datatops, check out the [Data Productivity - Beyond DevOps and dbt](https://datarecce.io/firesidechat/) Fireside Chat between the founders of Recce and Datacoves.