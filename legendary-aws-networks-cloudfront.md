<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Website Delivery with CloudFront

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-networks-cloudfront)

**Author:** Anas Sabir  
**Email:** anassabir008@gmail.com

---

## Website Delivery with CloudFront

![Image](http://learn.nextwork.org/daring_magenta_fierce_freshwater_clam/uploads/aws-networks-cloudfront_1dddddwe)

---

## Introducing Today's Project!

In this project, I will demonstrate how to use CloudFront to deliver a website. I'm doing this project to learn CDNs (Content Delivery Networks), and to set up the presentation tier of a three-tier architecture.

### Tools and concepts

Services I used were Amazon S3 and Amazon CloudFront. Key concepts I learnt include content delivery network (CDN), caching, Origin Access Control (OAC), and updating S3 bucket policies to secure private content delivery.

### Project reflection

This project took me approximately 40 minutes to complete. The most challenging part was configuring the CloudFront OAC and updating the S3 bucket policies to resolve the access denied error. It was most rewarding to see my website successfully deliver globally through Amazon CloudFront!

I chose to do this project today because I wanted to learn how to securely and rapidly distribute a static website using Amazon S3 and Amazon CloudFront.

---

## Set Up S3 and Website Files

I started the project by creating an S3 bucket to upload ti it my website's files I can't use CloudFront for this task because it's not used for storage.

The three files that make up my website are index.html, style.css, script.js and images.

I validated my website files by opening the index.html file locally in my web browser to confirm that the web page rendered and loaded correctly

![Image](http://learn.nextwork.org/daring_magenta_fierce_freshwater_clam/uploads/aws-networks-cloudfront_qgo7wcd3)

---

## Exploring Amazon CloudFront

Amazon CloudFront is a Content Delivery Network (CDN), which means it speeds up the distribution of my static and dynamic web content. Businesses and developers use CloudFront because it speeds up theire website performence.

To use Amazon CloudFront, you set up distributions, which are a sets of instructions that tells CloudFront how to deliver the content.

It specifies where website's files are stored (called the origin), how they should be cached, and other delivery settings like security standards. I set up a distribution The origin is the s3 bucket i created

My CloudFront distribution's default root object is index.html. This means that when a user navigates to the main domain of my CloudFront distribution without requesting a specific file path, CloudFront will automatically fetch and deliver the index.html file as the homepage of my website.

![Image](http://learn.nextwork.org/daring_magenta_fierce_freshwater_clam/uploads/aws-networks-cloudfront_qgo7wcdt)

---

## Handling Access Issues

When I tried visiting my distributed website, I ran into an access denied error because by default, S3 buckets are private. CloudFront needs explicit permission to access the files in your bucket.

My distribution's origin access settings were set to Public. This caused the access denied error because the S3 bucket and its files are private by default, and simply setting the origin access to public in CloudFront did not grant the CloudFront distribution the actual permission to read the private objects.

To resolve the error, I set up origin access control (OAC). OAC is a special user for CloudFront that prevents this. An OAC lets me keep my S3 bucket and objects not publicly accessible, while still making sure they can be accessed through CloudFront.

![Image](http://learn.nextwork.org/daring_magenta_fierce_freshwater_clam/uploads/aws-networks-cloudfront_egrhntyu)

---

## Updating S3 Permissions

Once I set up my OAC, I still needed to update my bucket policy because the S3 bucket is private by default. Creating the OAC in CloudFront does not automatically change permissions on the S3 side; the bucket policy must be explicitly updated to grant the OAC permission to retrieve the bucket's content.

Creating an OAC automatically gives me a policy I could copy, which grants the CloudFront service principal permission to perform s3:GetObject actions on the S3 bucket. This allows CloudFront to securely retrieve and serve my website files while keeping the bucket private from direct public access.

![Image](http://learn.nextwork.org/daring_magenta_fierce_freshwater_clam/uploads/aws-networks-cloudfront_eg98ntyu)

---

## S3 vs CloudFront for Hosting

---

## S3 vs CloudFront Load Times

---

---
