# Gen AI Intelligent Document Processing (GenAIIDP)

Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved.
SPDX-License-Identifier: MIT-0

**Questions?** [![Ask DeepWiki](https://deepwiki.com/badge.svg)](https://deepwiki.com/aws-solutions-library-samples/accelerated-intelligent-document-processing-on-aws)

## Table of Contents

- [Introduction](#introduction)
- [Alternative Implementations](#alternative-implementations)
- [Key Features](#key-features)
- [Architecture Overview](#architecture-overview)
- [Quick Start](#quick-start)
  - [Processing Your First Document](#processing-your-first-document)
- [Updating an Existing Deployment](#updating-an-existing-deployment)
- [Detailed Documentation](#detailed-documentation)
  - [Core Documentation](#core-documentation)
  - [Processing Patterns](#processing-patterns)
  - [Python Development](#python-development)
  - [Planning & Operations](#planning--operations)
- [Contributing](#contributing)
- [License](#license)

## Key Features

- **Serverless Architecture**: Built entirely on AWS serverless technologies including Lambda, Step Functions, SQS, and DynamoDB
- **Modular, pluggable patterns**: Pre-built processing patterns using state-of-the-art models and AWS services
- **Command Line Interface**: Programmatic batch processing with evaluation framework and analytics integration
- **Advanced Classification**: Support for page-level and holistic document packet classification
- **Few Shot Example Support**: Improve accuracy through example-based prompting
- **Custom Business Logic Integration**: Inject custom prompt generation logic via Lambda functions for specialized document processing
- **High Throughput Processing**: Handles large volumes of documents through intelligent queuing
- **Built-in Resilience**: Comprehensive error handling, retries, and throttling management
- **Cost Optimization**: Pay-per-use pricing model with built-in controls
- **Comprehensive Monitoring**: Rich CloudWatch dashboard with detailed metrics and logs
- **Web User Interface**: Modern UI for inspecting document workflow status and results
- **Human-in-the-Loop (HITL)**: Amazon A2I integration for human review workflows (Pattern 1 & Pattern 2)
  - **Note**: When deploying multiple patterns with HITL, reuse existing private workteam ARN due to AWS account limits
- **AI-Powered Evaluation**: Framework to assess accuracy against baseline data
- **Extraction Confidence Assessment**: LLM-powered assessment of extraction confidence with multimodal document analysis
- **Document Knowledge Base Query**: Ask questions about your processed documents
- **IDP Accelerator Help Chat Bot**: Ask questions about the IDP code base or features
- **MCP Integration**: Model Context Protocol integration enabling external applications like Amazon Quick Suite to access IDP data and analytics through AWS Bedrock AgentCore Gateway

## Architecture Overview

![Architecture Diagram](./images/IDP.drawio.png)

The solution uses a modular architecture with nested CloudFormation stacks to support multiple document processing patterns while maintaining common infrastructure for queueing, tracking, and monitoring.

Current patterns include:
- Pattern 1: Packet or Media processing with Bedrock Data Automation (BDA)
- Pattern 2: OCR → Bedrock Classification (page-level or holistic) → Bedrock Extraction
- Pattern 3: OCR → UDOP Classification (SageMaker) → Bedrock Extraction

### Processing Your First Document

After deployment, choose the processing method that fits your use case:

#### Method 1: Web UI (Interactive)

1. Open the Web UI URL from CloudFormation stack Outputs
2. Log in and click "Upload Document"
3. Upload a sample document:
   - For Patterns 1 & 2: [samples/lending_package.pdf](./samples/lending_package.pdf)
   - For Pattern 3: [samples/rvl_cdip_package.pdf](./samples/rvl_cdip_package.pdf)
4. Monitor processing and view results in the dashboard

#### Method 2: Direct S3 Upload (Simple)

1. Upload to the InputBucket (URL in CloudFormation Outputs)
2. Monitor via Step Functions console
3. Results appear in OutputBucket automatically

#### Method 3: IDP CLI (Batch/Programmatic)

For batch processing, automation, or evaluation workflows:

```bash
# Install CLI
cd idp_cli && pip install -e .

# Process documents
idp-cli run-inference \
    --stack-name <your-stack-name> \
    --dir ./samples/ \
    --monitor

# Download results
idp-cli download-results \
    --stack-name <your-stack-name> \
    --batch-id <batch-id> \
    --output-dir ./results/
```

**See [IDP CLI Documentation](./docs/idp-cli.md)** for:
- CLI-based stack deployment and updates
- Batch document processing
- Complete evaluation workflows with baselines
- Athena and Agent Analytics integration
- CI/CD integration examples

See the [Deployment Guide](./docs/deployment.md#testing-the-solution) for more detailed testing instructions.

IMPORTANT: If you have not previously done so, you must [request access](https://docs.aws.amazon.com/bedrock/latest/userguide/model-access.html) to the following Amazon Bedrock models:
- Amazon: All Nova models, plus Titan Text Embeddings V2
- Anthropic: Claude 3.x models, Claude 4.x models

