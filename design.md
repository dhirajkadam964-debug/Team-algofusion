# Arch-Intelligence Design Document

## System Architecture

### High-Level Architecture
The system follows a serverless architecture pattern using AWS services:

```
Frontend (React/Next.js) 
    ↓
API Gateway 
    ↓
Lambda Functions 
    ↓
Amazon Bedrock (Claude 3.5 Sonnet) + Amazon Q Business
    ↓
S3 (File Storage) + Bedrock Knowledge Bases (RAG)
```

### Core Components

#### 1. Architecture X-Ray Service
**Purpose**: Analyze codebases and generate interactive logic diagrams

**Components**:
- **Code Parser Lambda**: Analyzes uploaded code files and extracts structure
- **Diagram Generator Lambda**: Uses Amazon Bedrock to create architectural representations
- **Interactive Renderer**: Frontend component for displaying and interacting with diagrams

**Data Flow**:
1. User uploads code → S3 bucket (temporary, encrypted)
2. Code Parser Lambda processes files → extracts components, dependencies, data flows
3. Diagram Generator Lambda → sends structured data to Bedrock → receives diagram specification
4. Frontend renders interactive diagram with click-to-code functionality

#### 2. Socratic Tutor Service  
**Purpose**: Provide guided debugging assistance through leading questions

**Components**:
- **Context Analyzer Lambda**: Understands user's code context and error description
- **Question Generator Lambda**: Uses Amazon Q Business to formulate leading questions
- **Progress Tracker**: Maintains conversation state and learning progress

**Data Flow**:
1. User describes bug/error → Context Analyzer processes with code context
2. Question Generator uses Amazon Q Business → generates contextual leading questions
3. User responses tracked → system adjusts question difficulty and approach
4. Progress stored in DynamoDB for session continuity

#### 3. Documentation Linking Service
**Purpose**: Automatically surface relevant technical documentation

**Components**:
- **Technology Detector Lambda**: Identifies frameworks, libraries, and technologies in code
- **RAG Engine**: Uses Amazon Bedrock Knowledge Bases to find relevant documentation
- **Link Curator**: Prioritizes and formats documentation links

**Data Flow**:
1. Code analysis identifies technologies → Technology Detector extracts tech stack
2. RAG Engine queries Bedrock Knowledge Bases → retrieves relevant documentation
3. Link Curator ranks and formats results → presents contextual documentation links

### Data Models

#### Code Analysis Result
```typescript
interface CodeAnalysis {
  id: string;
  userId: string;
  timestamp: Date;
  components: Component[];
  dependencies: Dependency[];
  dataFlows: DataFlow[];
  techStack: Technology[];
}

interface Component {
  name: string;
  type: 'function' | 'class' | 'module' | 'service';
  location: FileLocation;
  dependencies: string[];
  complexity: number;
}
```

#### Socratic Session
```typescript
interface SocraticSession {
  sessionId: string;
  userId: string;
  problemDescription: string;
  codeContext: CodeContext;
  questionHistory: Question[];
  currentLevel: 'beginner' | 'intermediate' | 'advanced';
  progress: number;
}

interface Question {
  id: string;
  text: string;
  type: 'diagnostic' | 'analytical' | 'solution-oriented';
  userResponse?: string;
  timestamp: Date;
}
```
### Security Design

#### Authentication & Authorization
- AWS Cognito for user authentication
- JWT tokens for session management
- Role-based access control for different user types

#### Data Protection
- All code uploads encrypted in transit and at rest
- Temporary S3 storage with automatic deletion after analysis
- No permanent storage of user code
- VPC isolation for Lambda functions

#### Privacy Controls
- User consent for code analysis
- Data retention policies (24-hour maximum for temporary files)
- Audit logging for all user interactions

### AI Integration Strategy

#### Amazon Bedrock Integration
- **Model**: Claude 3.5 Sonnet for code analysis and diagram generation
- **Prompt Engineering**: Structured prompts for consistent architectural analysis
- **Rate Limiting**: Implement throttling to manage costs and ensure availability

#### Amazon Q Business Integration  
- **Knowledge Base**: Curated technical documentation and debugging methodologies
- **Question Generation**: Context-aware prompts for Socratic questioning
- **Learning Adaptation**: Track user responses to adjust question complexity

#### RAG Implementation
- **Knowledge Base**: Amazon Bedrock Knowledge Bases with verified technical sources
- **Document Ingestion**: Automated pipeline for updating documentation sources
- **Relevance Scoring**: Rank documentation by relevance to current code context

### Frontend Design

#### Technology Stack
- **Framework**: Next.js 14 with TypeScript
- **UI Library**: Tailwind CSS with Headless UI components
- **Diagram Rendering**: D3.js or React Flow for interactive diagrams
- **State Management**: Zustand for client-side state

#### User Interface Components
- **Code Upload Interface**: Drag-and-drop with progress indicators
- **Interactive Diagram Viewer**: Zoomable, clickable architectural diagrams
- **Socratic Chat Interface**: Clean chat UI with syntax highlighting
- **Documentation Sidebar**: Contextual documentation links and previews

### Performance Considerations

#### Caching Strategy
- CloudFront CDN for static assets
- API Gateway caching for common requests
- Client-side caching for diagram data

#### Optimization Techniques
- Lambda function warming to reduce cold starts
- Streaming responses for large diagram generation
- Progressive loading for complex diagrams
- Debounced API calls for real-time features

### Monitoring & Observability

#### Metrics Collection
- AWS CloudWatch for system metrics
- Custom metrics for user engagement and learning progress
- Error tracking and alerting

#### Logging Strategy
- Structured logging across all Lambda functions
- User interaction tracking (anonymized)
- Performance monitoring for AI service calls

## Correctness Properties

### Property 1: Code Privacy Guarantee
**Requirement**: User code must remain private and isolated (Requirements 4.1, 4.2)
**Property**: For any user code upload, the system must not store code permanently and must ensure isolation between users
**Test Strategy**: Verify temporary storage deletion and user isolation

### Property 2: Response Time Consistency
**Requirement**: System performance must meet specified thresholds (Requirements 5.1-5.3)
**Property**: All user-facing operations must complete within their specified time limits
**Test Strategy**: Load testing with response time monitoring

### Property 3: Architecture Diagram Accuracy
**Requirement**: Generated diagrams must accurately represent code structure (Requirements 1.2-1.4)
**Property**: For any valid codebase, the generated diagram must correctly show component relationships and data flows
**Test Strategy**: Compare generated diagrams against known architectural patterns

### Property 4: Socratic Question Relevance
**Requirement**: Debugging questions must be contextually relevant (Requirements 2.2-2.4)
**Property**: Generated questions must relate to the user's specific code context and error description
**Test Strategy**: Evaluate question relevance using predefined code scenarios

## Testing Framework
- **Unit Tests**: Jest for Lambda functions and frontend components
- **Integration Tests**: AWS SAM for serverless integration testing
- **Property-Based Tests**: fast-check for JavaScript/TypeScript property testing
- **End-to-End Tests**: Playwright for full user journey testing is it ready to submit



***This isn't just another coding tool - it's a learning platform that helps you think like a senior developer.*** 