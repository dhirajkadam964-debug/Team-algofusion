# Team-algofusion
"An AWS-powered Socratic mentoring platform for Indian engineering students. Uses Amazon Bedrock &amp; RAG to transform code into interactive architecture maps and guided learning journeys to escape 'Tutorial Hell'."  : aws-ai-for-bharat amazon-bedrock socratic-learning nextjs serverless rag system-design
 Arch-Intelligence
Think Like a Senior Developer, Not a Code Copier.
Arch-Intelligence is an AI-powered productivity and learning platform built for the AWS AI for Bharat Hackathon. It solves the problem of "Tutorial Hell" by helping students visualize the why behind their code through interactive architectural mapping and Socratic mentorship.

*The Problem
In Bharat’s rapidly growing tech landscape, 1.5M+ engineers graduate annually. Many fall into "Tutorial Hell"—the ability to copy code without understanding the underlying system architecture or data flow.

*Key Features
Architecture X-Ray: Automatically generates interactive logic diagrams from your codebase using Claude 3.5 Sonnet.

Socratic Debugging: Powered by Amazon Q Business, the tutor doesn't give answers; it asks leading questions to build your architectural intuition.

Contextual RAG: Uses Bedrock Knowledge Bases to ground all advice in official AWS best practices and technical documentation.

Bharat-Scale Engineering: A 100% Serverless stack optimized for a unit cost of ₹2.80 per student.

*How it Works (The Design)
As detailed in our Design Document, the system utilizes a zero-trust serverless flow:

Ingestion: Code is uploaded to an encrypted S3 Bucket.

Analysis: An AWS Lambda parser extracts components and dependencies.

Visualization: Amazon Bedrock generates a Mermaid.js specification rendered in a Next.js dashboard.

Mentorship: Users interact with the Socratic Tutor Service to explore their system's logic.

* Tech Stack
Frontend: Next.js, Tailwind CSS, Mermaid.js

Intelligence: Amazon Bedrock (Claude 3.5 Sonnet), Amazon Q Business

Data: Amazon S3, Bedrock Knowledge Bases (Vector Store)

Compute: AWS Lambda, Amazon API Gateway
