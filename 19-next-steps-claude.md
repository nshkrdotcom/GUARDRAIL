# Comprehensive Review of GUARDRAIL Security Framework

After thoroughly analyzing the GUARDRAIL security framework for Large Language Model applications, I'm impressed by its comprehensive approach to addressing the unique security challenges in LLM ecosystems, particularly those built on the Model Context Protocol (MCP).

## Key Strengths

1. **Multi-layered Defense Architecture** - The framework's five security layers (Information Gateway, Context Verification, Request Control, Execution Containment, and Audit/Monitoring) provide comprehensive protection against a wide range of threats.

2. **Flexible Deployment Models** - Offering embedded, gateway, and service mesh deployment options accommodates diverse organizational needs and existing infrastructure.

3. **Practical Security Innovations** - The framework introduces several innovative concepts:
   - Extensible Security Middleware (ESM) for pluggable security modules
   - Dynamic Security Context (DSC) for adaptive security responses
   - Protocol-level security annotations for message integrity
   - Lightweight Attestation Protocol (LAP) for environment verification
   - Adaptive Resource Quotas (ARQ) for dynamic resource control
   - Security Event Correlation and Reporting (SECR) for comprehensive monitoring

4. **Incremental Adoption Path** - The design explicitly acknowledges the need for progressive security implementation, allowing organizations to start with basic protections and evolve.

5. **Enterprise Integration** - The emergency response framework and compliance mappings demonstrate consideration for enterprise security requirements and regulatory standards.

## Opportunities for Enhancement

Based on my analysis, I recommend the following enhancements to strengthen GUARDRAIL:

### 1. Implementation Toolkit

**Recommended Action:** Develop a practical implementation toolkit with:
- Reference implementations for each core component
- Pre-built Docker containers for the gateway model
- Configuration templates for common deployment scenarios
- SDK libraries for multiple programming languages
- CI/CD integration examples

This would significantly lower the adoption barrier and provide concrete starting points for implementation teams.

### 2. Threat Intelligence Integration

**Recommended Action:** Create a threat intelligence framework specifically for LLM applications:
- LLM-specific attack pattern database
- Prompt injection attack signatures
- Known vulnerability repository
- Real-time threat feed integration
- Community-sourced threat indicators

This would enhance GUARDRAIL's ability to detect and respond to emerging threats targeting LLM applications.

### 3. Performance Optimization Guidelines

**Recommended Action:** Develop performance optimization guidelines addressing:
- Caching strategies for frequent security operations
- Selective security processing based on risk profiles
- Lightweight classification algorithms for high-throughput scenarios
- Benchmarking tools for measuring security overhead
- Hardware acceleration options for cryptographic operations

These guidelines would help organizations balance security and performance requirements effectively.

### 4. Privacy-Enhanced Security Controls

**Recommended Action:** Extend GUARDRAIL with privacy-enhancing technologies:
- Differential privacy mechanisms for sensitive data handling
- Homomorphic encryption support for protected computation
- Privacy-preserving federated learning capabilities
- Data minimization automation tools
- Consent management integration

This would address growing privacy concerns around LLM applications, particularly in regulated industries.

### 5. Cross-Model Security Standardization

**Recommended Action:** Develop standardized security interfaces for different LLM providers:
- Common security API abstractions
- Standard security metadata schemas
- Cross-provider attestation mechanisms
- Unified capability definitions
- Portable security policies

This would allow GUARDRAIL to provide consistent security across heterogeneous LLM environments.

## Next Steps Roadmap

I propose the following phased implementation roadmap:

### Phase 1: Foundation (0-3 months)
- Develop reference implementations of ESM and Protocol-Level Security Annotations
- Create initial documentation and developer guides
- Establish community governance structure
- Release security policy templates

### Phase 2: Core Components (3-6 months)
- Implement Dynamic Security Context (DSC) mechanisms
- Develop Lightweight Attestation Protocol (LAP)
- Build basic Security Event Correlation and Reporting (SECR)
- Create integration examples with common security tools

### Phase 3: Enterprise Features (6-12 months)
- Implement full Adaptive Resource Quotas (ARQ)
- Develop comprehensive audit and compliance reporting
- Create enterprise integration connectors (SIEM, IAM, etc.)
- Build advanced threat detection capabilities

### Phase 4: Advanced Capabilities (12+ months)
- Implement privacy-enhancing technologies
- Develop multi-tenant security isolation
- Build federated security models
- Create automated security testing frameworks

## Conclusion

GUARDRAIL represents a significant advancement in LLM application security, providing a comprehensive framework that addresses the unique challenges of these environments. By implementing the recommendations above and following the proposed roadmap, organizations can enhance the practical applicability and effectiveness of GUARDRAIL while maintaining its strong security foundation.

The framework's emphasis on incremental adoption is particularly valuable, allowing organizations to start with core security features and progressively enhance their security posture as their LLM applications mature and expand.

Would you like me to elaborate on any specific aspect of this review or provide more detailed recommendations for a particular area of enhancement?











# GUARDRAIL Phase 1 Implementation: Foundation (0-3 months)

## Executive Summary

Phase 1 focuses on establishing the fundamental building blocks of GUARDRAIL by implementing two core innovations: the Extensible Security Middleware (ESM) and Protocol-Level Security Annotations. This phase lays the groundwork for all subsequent security layers while providing immediate security benefits with minimal integration friction. The deliverables from this phase will enable early adopters to begin securing their Model Context Protocol (MCP) implementations while establishing the community and governance foundation for sustained development.

## 1. Extensible Security Middleware (ESM) Implementation






















```typescript
/**
 * Extensible Security Middleware (ESM) Core Architecture
 * 
 * The ESM provides a pluggable architecture within MCP client and server 
 * implementations, allowing for customized security processing of messages.
 * It sits between the MCP protocol layer and the transport layer.
 */

// Core interfaces

/**
 * Interface for a security module that can process MCP messages
 */
export interface SecurityModule {
  /** Unique identifier for the module */
  id: string;
  
  /** Human-readable name for the module */
  name: string;
  
  /** Module version */
  version: string;
  
  /** Module initialization with configuration and security context */
  initialize(config: ModuleConfig, context: SecurityContext): Promise<void>;
  
  /** Validate an incoming message before further processing */
  validate?(message: MCPMessage): Promise<ValidationResult>;
  
  /** Transform a message (can modify content based on security policies) */
  transform?(message: MCPMessage): Promise<MCPMessage>;
  
  /** Process a message before it's sent to the transport */
  preSend?(message: MCPMessage): Promise<MCPMessage>;
  
  /** Process a message after it's received from the transport */
  postReceive?(message: MCPMessage): Promise<MCPMessage>;
  
  /** Module cleanup when shutting down */
  cleanup?(): Promise<void>;
}

/**
 * Interface for the plugin manager that coordinates security modules
 */
export interface PluginManager {
  /** Register a security module */
  registerModule(module: SecurityModule): void;
  
  /** Unregister a security module */
  unregisterModule(moduleId: string): void;
  
  /** Get a module by its ID */
  getModule(moduleId: string): SecurityModule | undefined;
  
  /** Get all registered modules */
  getAllModules(): SecurityModule[];
  
  /** Configure the processing pipeline */
  configurePipeline(config: PipelineConfig): void;
  
  /** Process a message through the entire pipeline */
  processMessage(message: MCPMessage, direction: 'inbound' | 'outbound'): Promise<MCPMessage>;
}

/**
 * Interface for the dynamic security context shared between modules
 */
export interface SecurityContext {
  /** Current trust score (0.0-1.0) */
  trustScore: number;
  
  /** Current threat level (low, medium, high, critical) */
  threatLevel: ThreatLevel;
  
  /** Active security capabilities */
  capabilities: Set<string>;
  
  /** Security session data */
  sessionData: Map<string, any>;
  
  /** Event history for the current session */
  eventHistory: SecurityEvent[];
  
  /** Update the trust score */
  updateTrustScore(adjustment: number, reason: string): void;
  
  /** Update the threat level */
  updateThreatLevel(newLevel: ThreatLevel, reason: string): void;
  
  /** Add a security capability */
  addCapability(capability: string): void;
  
  /** Remove a security capability */
  removeCapability(capability: string): void;
  
  /** Log a security event */
  logEvent(event: SecurityEvent): void;
}

// Configuration types

/**
 * Configuration for a security module
 */
export interface ModuleConfig {
  /** Module-specific options */
  options: Record<string, any>;
  
  /** Whether the module is enabled */
  enabled: boolean;
  
  /** Order in the pipeline (lower numbers execute earlier) */
  order: number;
}

/**
 * Configuration for the processing pipeline
 */
export interface PipelineConfig {
  /** Module configurations, keyed by module ID */
  modules: Record<string, ModuleConfig>;
  
  /** Default action when validation fails */
  validationFailureAction: 'block' | 'warn' | 'ignore';
  
  /** Whether to continue processing after a module error */
  continueOnError: boolean;
}

// Result types

/**
 * Result of a validation operation
 */
export interface ValidationResult {
  /** Whether validation passed */
  valid: boolean;
  
  /** Validation error, if any */
  error?: string;
  
  /** Validation severity (info, warning, error) */
  severity: 'info' | 'warning' | 'error';
}

// Implementation of the ESM for MCP

/**
 * Main ESM class that integrates with MCP implementations
 */
export class ExtensibleSecurityMiddleware {
  private pluginManager: PluginManager;
  private securityContext: SecurityContext;
  
  constructor(config: ESMConfig) {
    this.pluginManager = new DefaultPluginManager();
    this.securityContext = new DefaultSecurityContext(config.security);
    
    // Register built-in modules
    this.registerBuiltInModules();
    
    // Configure the pipeline
    this.pluginManager.configurePipeline(config.pipeline);
  }
  
  /**
   * Process an outbound message (from application to transport)
   */
  async processOutbound(message: MCPMessage): Promise<MCPMessage> {
    return this.pluginManager.processMessage(message, 'outbound');
  }
  
  /**
   * Process an inbound message (from transport to application)
   */
  async processInbound(message: MCPMessage): Promise<MCPMessage> {
    return this.pluginManager.processMessage(message, 'inbound');
  }
  
  /**
   * Get the current security context
   */
  getSecurityContext(): SecurityContext {
    return this.securityContext;
  }
  
  /**
   * Register additional security modules
   */
  registerModule(module: SecurityModule, config?: ModuleConfig): void {
    module.initialize(config || { 
      enabled: true, 
      order: 100, 
      options: {} 
    }, this.securityContext);
    
    this.pluginManager.registerModule(module);
  }
  
  /**
   * Register built-in security modules
   */
  private registerBuiltInModules(): void {
    // Register validation module
    this.registerModule(new ValidationModule(), { 
      enabled: true, 
      order: 10, 
      options: { strictMode: true } 
    });
    
    // Register classification module
    this.registerModule(new ClassificationModule(), { 
      enabled: true, 
      order: 20, 
      options: { defaultClassification: 'INTERNAL' } 
    });
    
    // Register transformation module
    this.registerModule(new TransformationModule(), { 
      enabled: true, 
      order: 30, 
      options: { transformations: ['sanitize', 'redact'] } 
    });
  }
}

// Example of MCP client integration

/**
 * Example of how to integrate ESM with an MCP client
 */
export function createSecureClient(options: MCPClientOptions): MCPClient {
  // Create standard MCP client
  const mcpClient = new MCPClient(options);
  
  // Create ESM instance
  const esm = new ExtensibleSecurityMiddleware({
    security: {
      initialTrustScore: 0.8,
      initialThreatLevel: 'low',
      capabilities: ['basic_access']
    },
    pipeline: {
      modules: {
        // Configuration for built-in modules
      },
      validationFailureAction: 'block',
      continueOnError: false
    }
  });
  
  // Intercept outbound messages
  mcpClient.setOutboundInterceptor(async (message) => {
    return await esm.processOutbound(message);
  });
  
  // Intercept inbound messages
  mcpClient.setInboundInterceptor(async (message) => {
    return await esm.processInbound(message);
  });
  
  return mcpClient;
}
```

```typescript
/**
 * Example implementations of ESM security modules
 */

import { 
  SecurityModule, 
  ModuleConfig, 
  SecurityContext, 
  ValidationResult, 
  MCPMessage 
} from './esm-architecture';

/**
 * Classification Module
 * 
 * Analyzes and classifies messages based on content sensitivity
 */
export class ClassificationModule implements SecurityModule {
  id = 'guardrail.classification';
  name = 'Content Classification Module';
  version = '1.0.0';
  
  private context!: SecurityContext;
  private config!: ModuleConfig;
  private classificationRules: ClassificationRule[] = [];
  
  async initialize(config: ModuleConfig, context: SecurityContext): Promise<void> {
    this.config = config;
    this.context = context;
    
    // Load classification rules from configuration
    this.loadClassificationRules();
  }
  
  async transform(message: MCPMessage): Promise<MCPMessage> {
    // Skip if message already has classification
    if (message.security?.classification) {
      return message;
    }
    
    // Analyze message content to determine classification
    const classification = this.classifyContent(message);
    
    // Add classification to message
    return {
      ...message,
      security: {
        ...message.security,
        classification: classification
      }
    };
  }
  
  private classifyContent(message: MCPMessage): 'PUBLIC' | 'INTERNAL' | 'SENSITIVE' | 'RESTRICTED' {
    let highestScore = 0;
    let classification: 'PUBLIC' | 'INTERNAL' | 'SENSITIVE' | 'RESTRICTED' = 
      this.config.options.defaultClassification || 'INTERNAL';
    
    // Convert message to string for pattern matching
    const content = JSON.stringify(message.params || {});
    
    // Apply each classification rule
    for (const rule of this.classificationRules) {
      let score = 0;
      
      // Check each pattern in the rule
      for (const pattern of rule.patterns) {
        if (this.matchesPattern(content, pattern)) {
          score += pattern.weight;
        }
      }
      
      // If score exceeds threshold and is higher than current highest
      if (score >= rule.threshold && score > highestScore) {
        highestScore = score;
        classification = rule.classification;
      }
    }
    
    return classification;
  }
  
  private matchesPattern(content: string, pattern: { type: string, value: string }): boolean {
    switch (pattern.type) {
      case 'regex':
        return new RegExp(pattern.value, 'i').test(content);
      case 'exact':
        return content.includes(pattern.value);
      case 'semantic':
        // Simplified implementation - in practice, this would use more sophisticated analysis
        return content.toLowerCase().includes(pattern.value.toLowerCase());
      default:
        return false;
    }
  }
  
  private loadClassificationRules(): void {
    // Load rules from configuration or default rules
    this.classificationRules = this.config.options.rules || [
      {
        id: 'personal-data',
        name: 'Personal Data',
        patterns: [
          { type: 'regex', value: '\\b[A-Z0-9._%+-]+@[A-Z0-9.-]+\\.[A-Z]{2,}\\b', weight: 0.7 },
          { type: 'regex', value: '\\b\\d{3}-\\d{2}-\\d{4}\\b', weight: 0.9 }, // SSN
          { type: 'semantic', value: 'personal', weight: 0.5 }
        ],
        threshold: 0.7,
        classification: 'SENSITIVE'
      },
      {
        id: 'financial-data',
        name: 'Financial Data',
        patterns: [
          { type: 'regex', value: '\\b(?:\\d[ -]*?){13,16}\\b', weight: 0.8 }, // Credit card
          { type: 'semantic', value: 'financial', weight: 0.6 },
          { type: 'semantic', value: 'account', weight: 0.4 }
        ],
        threshold: 0.6,
        classification: 'RESTRICTED'
      }
    ];
  }
}

/**
 * Validation Module
 * 
 * Validates message structure and content against security policies
 */
export class ValidationModule implements SecurityModule {
  id = 'guardrail.validation';
  name = 'Message Validation Module';
  version = '1.0.0';
  
  private context!: SecurityContext;
  private config!: ModuleConfig;
  
  async initialize(config: ModuleConfig, context: SecurityContext): Promise<void> {
    this.config = config;
    this.context = context;
  }
  
  async validate(message: MCPMessage): Promise<ValidationResult> {
    // Basic validation checks
    
    // 1. Check for required fields
    if (!message.jsonrpc || message.jsonrpc !== '2.0') {
      return {
        valid: false,
        error: 'Invalid JSON-RPC version',
        severity: 'error'
      };
    }
    
    // 2. Check for potential injection attacks
    const injectionDetected = this.detectInjectionAttempts(message);
    if (injectionDetected) {
      // Lower trust score upon detecting potential attack
      this.context.updateTrustScore(-0.2, 'Potential injection attack detected');
      this.context.updateThreatLevel('medium', 'Suspicious content pattern');
      
      return {
        valid: false,
        error: 'Potential injection attack detected',
        severity: 'error'
      };
    }
    
    // 3. Size constraints
    const messageSizeBytes = new TextEncoder().encode(JSON.stringify(message)).length;
    const maxSize = this.config.options.maxSizeBytes || 1024 * 1024; // 1MB default
    
    if (messageSizeBytes > maxSize) {
      return {
        valid: false,
        error: `Message size exceeds maximum (${messageSizeBytes} > ${maxSize} bytes)`,
        severity: 'error'
      };
    }
    
    // 4. Check security field if required in strict mode
    if (this.config.options.strictMode && !message.security) {
      return {
        valid: false,
        error: 'Security field is required in strict mode',
        severity: 'error'
      };
    }
    
    return { valid: true, severity: 'info' };
  }
  
  private detectInjectionAttempts(message: MCPMessage): boolean {
    // Convert message params to string for analysis
    const content = JSON.stringify(message.params || {});
    
    // Simple pattern matching for common injection patterns
    const suspiciousPatterns = [
      /\${.+}/i,                    // Template injection
      /<script\b[^<]*(?:(?!<\/script>)<[^<]*)*<\/script>/i, // Script tags
      /\b(exec|eval|function\(|setTimeout|setInterval)\(/i, // Javascript execution
      /\b(SELECT|INSERT|UPDATE|DELETE|DROP|UNION|ALTER)\b/i // SQL injection
    ];
    
    return suspiciousPatterns.some(pattern => pattern.test(content));
  }
}

/**
 * Transformation Module
 * 
 * Applies security transformations to messages based on policies
 */
export class TransformationModule implements SecurityModule {
  id = 'guardrail.transformation';
  name = 'Content Transformation Module';
  version = '1.0.0';
  
  private context!: SecurityContext;
  private config!: ModuleConfig;
  private transformations: Map<string, (content: any) => any> = new Map();
  
  async initialize(config: ModuleConfig, context: SecurityContext): Promise<void> {
    this.config = config;
    this.context = context;
    
    // Register built-in transformations
    this.registerTransformations();
  }
  
  async transform(message: MCPMessage): Promise<MCPMessage> {
    // Skip transformation if not needed based on classification
    const classification = message.security?.classification || 'INTERNAL';
    
    // Determine which transformations to apply based on classification
    const transformsToApply = this.getTransformationsForClassification(classification);
    if (transformsToApply.length === 0) {
      return message;
    }
    
    // Create a deep copy of the message to avoid modifying the original
    const transformedMessage = JSON.parse(JSON.stringify(message));
    
    // Apply each transformation in sequence
    for (const transformName of transformsToApply) {
      const transform = this.transformations.get(transformName);
      if (transform) {
        // Apply transformation to params
        if (transformedMessage.params) {
          transformedMessage.params = transform(transformedMessage.params);
        }
        
        // Record the transformation in the security metadata
        if (!transformedMessage.security) {
          transformedMessage.security = {};
        }
        
        if (!transformedMessage.security.transformations) {
          transformedMessage.security.transformations = [];
        }
        
        transformedMessage.security.transformations.push(transformName);
      }
    }
    
    return transformedMessage;
  }
  
  private getTransformationsForClassification(classification: string): string[] {
    // Policy for which transformations to apply based on classification
    switch (classification) {
      case 'RESTRICTED':
        return ['redact', 'sanitize'];
      case 'SENSITIVE':
        return ['redact_pii', 'sanitize'];
      case 'INTERNAL':
        return ['sanitize'];
      case 'PUBLIC':
      default:
        return [];
    }
  }
  
  private registerTransformations(): void {
    // Register sanitize transformation
    this.transformations.set('sanitize', (content: any) => {
      if (typeof content === 'string') {
        // Basic HTML sanitization
        return content
          .replace(/<script\b[^<]*(?:(?!<\/script>)<[^<]*)*<\/script>/gi, '')
          .replace(/<iframe\b[^<]*(?:(?!<\/iframe>)<[^<]*)*<\/iframe>/gi, '')
          .replace(/javascript:/gi, 'blocked:');
      } else if (typeof content === 'object' && content !== null) {
        // Recursively sanitize object properties
        const result: Record<string, any> = {};
        for (const [key, value] of Object.entries(content)) {
          result[key] = this.transformations.get('sanitize')!(value);
        }
        return result;
      } else if (Array.isArray(content)) {
        // Recursively sanitize array elements
        return content.map(item => this.transformations.get('sanitize')!(item));
      } else {
        // Primitive values pass through unchanged
        return content;
      }
    });
    
    // Register redact_pii transformation
    this.transformations.set('redact_pii', (content: any) => {
      if (typeof content === 'string') {
        // Redact common PII patterns
        return content
          .replace(/\b[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,}\b/gi, '[REDACTED EMAIL]')
          .replace(/\b(?:\d[ -]*?){13,16}\b/g, '[REDACTED CARD]') // Credit card numbers
          .replace(/\b\d{3}[-.]?\d{3}[-.]?\d{4}\b/g, '[REDACTED PHONE]') // Phone numbers
          .replace(/\b\d{3}[-]?\d{2}[-]?\d{4}\b/g, '[REDACTED SSN]'); // SSN
      } else if (typeof content === 'object' && content !== null) {
        // Recursively process object properties
        const result: Record<string, any> = {};
        for (const [key, value] of Object.entries(content)) {
          // Redact specific fields by name
          if (['ssn', 'socialSecurity', 'creditCard', 'password', 'secret'].includes(key.toLowerCase())) {
            result[key] = '[REDACTED]';
          } else {
            result[key] = this.transformations.get('redact_pii')!(value);
          }
        }
        return result;
      } else if (Array.isArray(content)) {
        // Recursively process array elements
        return content.map(item => this.transformations.get('redact_pii')!(item));
      } else {
        // Primitive values pass through unchanged
        return content;
      }
    });
    
    // Register complete redaction transformation
    this.transformations.set('redact', (content: any) => {
      if (typeof content === 'object' && content !== null) {
        // Replace object values with redacted indicator but keep structure
        const result: Record<string, any> = {};
        for (const key of Object.keys(content)) {
          if (Array.isArray(content[key])) {
            result[key] = [];
          } else if (typeof content[key] === 'object' && content[key] !== null) {
            result[key] = this.transformations.get('redact')!(content[key]);
          } else {
            result[key] = '[REDACTED]';
          }
        }
        return result;
      } else if (Array.isArray(content)) {
        // Replace array with empty array
        return [];
      } else if (typeof content === 'string') {
        // Replace string with redacted indicator
        return '[REDACTED]';
      } else {
        // Replace other values with null
        return null;
      }
    });
  }
}

/**
 * Transport Security Module
 * 
 * Handles encryption, integrity protection, and replay prevention
 */
export class TransportSecurityModule implements SecurityModule {
  id = 'guardrail.transport';
  name = 'Transport Security Module';
  version = '1.0.0';
  
  private context!: SecurityContext;
  private config!: ModuleConfig;
  private sequenceNumbers: Map<string, number> = new Map();
  
  async initialize(config: ModuleConfig, context: SecurityContext): Promise<void> {
    this.config = config;
    this.context = context;
  }
  
  async preSend(message: MCPMessage): Promise<MCPMessage> {
    // Add integrity token for outbound messages
    const securedMessage = { ...message };
    
    if (!securedMessage.security) {
      securedMessage.security = {};
    }
    
    // Add sequence number for replay protection
    const source = this.config.options.sourceId || 'unknown';
    const sequenceNumber = (this.sequenceNumbers.get(source) || 0) + 1;
    this.sequenceNumbers.set(source, sequenceNumber);
    
    securedMessage.security.source = source;
    securedMessage.security.sequence = sequenceNumber;
    
    // Add integrity hash (in a real implementation, this would use a proper HMAC)
    securedMessage.security.integrity = this.calculateIntegrityHash(securedMessage);
    
    return securedMessage;
  }
  
  async postReceive(message: MCPMessage): Promise<MCPMessage> {
    // Skip integrity check if no security field
    if (!message.security) {
      return message;
    }
    
    // Check for replay attacks using sequence numbers
    const source = message.security.source;
    const sequence = message.security.sequence;
    
    if (source && typeof sequence === 'number') {
      const lastSequence = this.sequenceNumbers.get(source) || 0;
      
      if (sequence <= lastSequence) {
        // This might be a replay attack, log and potentially reject
        this.context.logEvent({
          type: 'security.transport.replay_detected',
          severity: 'high',
          details: { source, sequence, lastSequence }
        });
        
        // Decrease trust score
        this.context.updateTrustScore(-0.3, 'Potential replay attack detected');
        
        // In strict mode, we would reject the message
        if (this.config.options.strictReplayProtection) {
          throw new Error(`Replay attack detected (${source}: ${sequence} <= ${lastSequence})`);
        }
      }
      
      // Update sequence number
      this.sequenceNumbers.set(source, sequence);
    }
    
    // Verify integrity
    if (message.security.integrity) {
      const originalIntegrity = message.security.integrity;
      
      // Make a copy without the integrity field for verification
      const messageForVerification = JSON.parse(JSON.stringify(message));
      delete messageForVerification.security.integrity;
      
      const calculatedIntegrity = this.calculateIntegrityHash(messageForVerification);
      
      if (calculatedIntegrity !== originalIntegrity) {
        // Integrity check failed, log and potentially reject
        this.context.logEvent({
          type: 'security.transport.integrity_failure',
          severity: 'high',
          details: { source, messageId: message.id }
        });
        
        // Decrease trust score significantly
        this.context.updateTrustScore(-0.5, 'Message integrity verification failed');
        
        // In strict mode, we would reject the message
        if (this.config.options.strictIntegrityChecks) {
          throw new Error('Message integrity verification failed');
        }
      }
    }
    
    return message;
  }
  
  private calculateIntegrityHash(message: any): string {
    // In a real implementation, this would use a cryptographic HMAC
    // This is just a simplified example
    const content = JSON.stringify(message);
    return 'sha256:' + this.simplifiedHash(content);
  }
  
  private simplifiedHash(content: string): string {
    // This is NOT a cryptographic hash - just for example purposes
    // A real implementation would use the Web Crypto API or a cryptographic library
    let hash = 0;
    for (let i = 0; i < content.length; i++) {
      const char = content.charCodeAt(i);
      hash = ((hash << 5) - hash) + char;
      hash = hash & hash; // Convert to 32bit integer
    }
    return Math.abs(hash).toString(16).padStart(8, '0');
  }
}
```

```typescript
{
  "openapi": "3.0.3",
  "info": {
    "title": "GUARDRAIL Protocol-Level Security Annotations",
    "description": "Specification for security metadata fields within MCP message structure",
    "version": "1.0.0",
    "contact": {
      "name": "GUARDRAIL Project",
      "url": "https://github.com/guardrail-project"
    }
  },
  "paths": {},
  "components": {
    "schemas": {
      "SecurityAnnotation": {
        "type": "object",
        "description": "Security metadata to be included in MCP messages",
        "required": ["classification"],
        "properties": {
          "classification": {
            "type": "string",
            "description": "Sensitivity level of the message content",
            "enum": ["PUBLIC", "INTERNAL", "SENSITIVE", "RESTRICTED"],
            "example": "INTERNAL"
          },
          "integrity": {
            "type": "string",
            "description": "Cryptographic hash to verify message integrity",
            "example": "sha256:a1b2c3d4e5f6..."
          },
          "source": {
            "type": "string",
            "description": "Identifier of the sender",
            "example": "client:123"
          },
          "sequence": {
            "type": "integer",
            "description": "Monotonically increasing sequence number to prevent replay attacks",
            "example": 42
          },
          "transformations": {
            "type": "array",
            "description": "Transformations that have been applied to the message content",
            "items": {
              "type": "string",
              "example": "redacted:pii"
            }
          }
        }
      },
      "MCPMessageWithSecurity": {
        "type": "object",
        "description": "Extended MCP message with security annotations",
        "required": ["jsonrpc", "id", "security"],
        "properties": {
          "jsonrpc": {
            "type": "string",
            "enum": ["2.0"],
            "description": "JSON-RPC version"
          },
          "method": {
            "type": "string",
            "description": "Method name"
          },
          "params": {
            "type": "object",
            "description": "Method parameters"
          },
          "id": {
            "oneOf": [
              { "type": "string" },
              { "type": "integer" }
            ],
            "description": "Request identifier"
          },
          "result": {
            "description": "Result data for a response"
          },
          "error": {
            "type": "object",
            "description": "Error information for failed requests"
          },
          "security": {
            "$ref": "#/components/schemas/SecurityAnnotation"
          }
        }
      },
      "ClassificationPolicy": {
        "type": "object",
        "description": "Policy defining classification levels and their handling",
        "properties": {
          "levels": {
            "type": "object",
            "additionalProperties": {
              "type": "object",
              "properties": {
                "description": {
                  "type": "string"
                },
                "flow_restrictions": {
                  "type": "array",
                  "items": {
                    "type": "object",
                    "properties": {
                      "source": {
                        "type": "string"
                      },
                      "destination": {
                        "type": "string"
                      },
                      "allowed": {
                        "type": "boolean"
                      },
                      "transformations": {
                        "type": "array",
                        "items": {
                          "type": "string"
                        }
                      }
                    }
                  }
                },
                "required_capability": {
                  "type": "string"
                },
                "audit_level": {
                  "type": "string",
                  "enum": ["none", "metadata", "full"]
                }
              }
            }
          },
          "default_level": {
            "type": "string"
          },
          "auto_classification": {
            "type": "boolean"
          }
        }
      }
    },
    "examples": {
      "RequestWithSecurity": {
        "summary": "Example MCP request with security annotations",
        "value": {
          "jsonrpc": "2.0",
          "method": "getResource",
          "params": {
            "resourceId": "doc123",
            "includeMetadata": true
          },
          "security": {
            "classification": "INTERNAL",
            "integrity": "sha256:8a7b3c2d1e...",
            "source": "client:app123",
            "sequence": 15
          },
          "id": 1
        }
      },
      "ResponseWithSecurity": {
        "summary": "Example MCP response with security annotations and transformations",
        "value": {
          "jsonrpc": "2.0",
          "result": {
            "resourceId": "doc123",
            "content": "This document contains [REDACTED] information.",
            "metadata": {
              "created": "2023-04-15T10:30:00Z",
              "owner": "[REDACTED EMAIL]"
            }
          },
          "security": {
            "classification": "SENSITIVE",
            "integrity": "sha256:9b8c7d6e5f...",
            "source": "server:resource-service",
            "sequence": 42,
            "transformations": ["redacted:pii"]
          },
          "id": 1
        }
      },
      "ClassificationPolicyExample": {
        "summary": "Example classification policy",
        "value": {
          "levels": {
            "PUBLIC": {
              "description": "Information safe for public disclosure",
              "flow_restrictions": [
                {
                  "source": "*",
                  "destination": "*",
                  "allowed": true
                }
              ],
              "required_capability": "none",
              "audit_level": "metadata"
            },
            "INTERNAL": {
              "description": "Information restricted to the application",
              "flow_restrictions": [
                {
                  "source": "mcp_client",
                  "destination": "mcp_server",
                  "allowed": true
                },
                {
                  "source": "mcp_server",
                  "destination": "mcp_client",
                  "allowed": true
                },
                {
                  "source": "*",
                  "destination": "external",
                  "allowed": false
                }
              ],
              "required_capability": "basic_access",
              "audit_level": "metadata"
            },
            "SENSITIVE": {
              "description": "Information requiring protection",
              "flow_restrictions": [
                {
                  "source": "mcp_client",
                  "destination": "mcp_server",
                  "allowed": true
                },
                {
                  "source": "mcp_server",
                  "destination": "mcp_client",
                  "allowed": true,
                  "transformations": ["redact_pii"]
                },
                {
                  "source": "*",
                  "destination": "external",
                  "allowed": false
                }
              ],
              "required_capability": "sensitive_access",
              "audit_level": "full"
            },
            "RESTRICTED": {
              "description": "Highly sensitive information",
              "flow_restrictions": [
                {
                  "source": "mcp_client",
                  "destination": "mcp_server",
                  "allowed": true,
                  "transformations": ["sanitize"]
                },
                {
                  "source": "mcp_server",
                  "destination": "mcp_client",
                  "allowed": true,
                  "transformations": ["redact"]
                },
                {
                  "source": "*",
                  "destination": "external",
                  "allowed": false
                }
              ],
              "required_capability": "restricted_access",
              "audit_level": "full"
            }
          },
          "default_level": "INTERNAL",
          "auto_classification": true
        }
      }
    }
  }
}
```

```typescript
/**
 * Protocol-Level Security Annotations Implementation
 * 
 * This module provides utilities for working with security annotations in MCP messages.
 */

// Type definitions for security annotations

/**
 * Classification level for message content
 */
export type ClassificationLevel = 'PUBLIC' | 'INTERNAL' | 'SENSITIVE' | 'RESTRICTED';

/**
 * Security metadata for MCP messages
 */
export interface SecurityAnnotation {
  /** Sensitivity level of the message content */
  classification: ClassificationLevel;
  
  /** Cryptographic hash to verify message integrity (optional) */
  integrity?: string;
  
  /** Identifier of the sender (optional) */
  source?: string;
  
  /** Sequence number to prevent replay attacks (optional) */
  sequence?: number;
  
  /** Transformations applied to the message content (optional) */
  transformations?: string[];
}

/**
 * Extended MCP message with security annotations
 */
export interface MCPMessageWithSecurity {
  /** JSON-RPC version */
  jsonrpc: '2.0';
  
  /** Method name (for requests) */
  method?: string;
  
  /** Method parameters (for requests) */
  params?: any;
  
  /** Response result (for responses) */
  result?: any;
  
  /** Error information (for error responses) */
  error?: any;
  
  /** Request identifier */
  id: string | number;
  
  /** Security metadata */
  security?: SecurityAnnotation;
}

/**
 * Flow restriction for a classification level
 */
export interface FlowRestriction {
  /** Source of the information flow */
  source: string;
  
  /** Destination of the information flow */
  destination: string;
  
  /** Whether the flow is allowed */
  allowed: boolean;
  
  /** Required transformations for the flow */
  transformations?: string[];
}

/**
 * Configuration for a classification level
 */
export interface ClassificationConfig {
  /** Human-readable description */
  description: string;
  
  /** Flow restrictions for this classification */
  flow_restrictions: FlowRestriction[];
  
  /** Required capability to access this classification */
  required_capability: string;
  
  /** Audit level for this classification */
  audit_level: 'none' | 'metadata' | 'full';
}

/**
 * Classification policy configuration
 */
export interface ClassificationPolicy {
  /** Classification levels and their configurations */
  levels: Record<ClassificationLevel, ClassificationConfig>;
  
  /** Default classification level */
  default_level: ClassificationLevel;
  
  /** Whether to automatically classify messages */
  auto_classification: boolean;
}

/**
 * Security Annotation Manager
 * 
 * Manages security annotations for MCP messages
 */
export class SecurityAnnotationManager {
  private policy: ClassificationPolicy;
  private sourceId: string;
  private sequenceCounter: number = 0;
  private integrityKey?: string;
  
  /**
   * Create a new SecurityAnnotationManager
   * 
   * @param policy Classification policy
   * @param sourceId Identifier for this source
   * @param integrityKey Key for integrity protection (optional)
   */
  constructor(
    policy: ClassificationPolicy,
    sourceId: string,
    integrityKey?: string
  ) {
    this.policy = policy;
    this.sourceId = sourceId;
    this.integrityKey = integrityKey;
  }
  
  /**
   * Add security annotations to an MCP message
   * 
   * @param message MCP message
   * @param classification Optional classification level (auto-detected if not provided)
   * @returns Message with security annotations
   */
  addSecurityAnnotations(
    message: MCPMessageWithSecurity,
    classification?: ClassificationLevel
  ): MCPMessageWithSecurity {
    // Use existing security field or create a new one
    const security: SecurityAnnotation = message.security || {
      classification: this.policy.default_level
    };
    
    // Set classification if provided or auto-classify if enabled
    if (classification) {
      security.classification = classification;
    } else if (!security.classification && this.policy.auto_classification) {
      security.classification = this.autoClassifyMessage(message);
    }
    
    // Add source identifier
    security.source = this.sourceId;
    
    // Add sequence number for replay protection
    security.sequence = ++this.sequenceCounter;
    
    // Create a new message with security annotations
    const annotatedMessage: MCPMessageWithSecurity = {
      ...message,
      security
    };
    
    // Add integrity protection if key is available
    if (this.integrityKey) {
      annotatedMessage.security!.integrity = this.calculateIntegrity(annotatedMessage);
    }
    
    return annotatedMessage;
  }
  
  /**
   * Verify security annotations on an MCP message
   * 
   * @param message MCP message with security annotations
   * @param expectedSource Optional expected source
   * @param lastSequence Optional last seen sequence number from this source
   * @returns Verification result
   */
  verifySecurityAnnotations(
    message: MCPMessageWithSecurity,
    expectedSource?: string,
    lastSequence?: number
  ): { 
    valid: boolean; 
    errors: string[]; 
    classification: ClassificationLevel 
  } {
    const errors: string[] = [];
    
    // Check if security field exists
    if (!message.security) {
      errors.push('Missing security annotations');
      return { 
        valid: false, 
        errors, 
        classification: this.policy.default_level 
      };
    }
    
    const { security } = message;
    
    // Verify classification
    if (!security.classification) {
      errors.push('Missing classification level');
    } else if (!this.policy.levels[security.classification]) {
      errors.push(`Invalid classification level: ${security.classification}`);
    }
    
    // Verify source if expected
    if (expectedSource && security.source !== expectedSource) {
      errors.push(`Source mismatch: expected ${expectedSource}, got ${security.source}`);
    }
    
    // Verify sequence for replay protection
    if (lastSequence !== undefined && security.sequence !== undefined) {
      if (security.sequence <= lastSequence) {
        errors.push(`Sequence number not increasing: ${security.sequence} <= ${lastSequence}`);
      }
    }
    
    // Verify integrity if present
    if (security.integrity && this.integrityKey) {
      const originalIntegrity = security.integrity;
      
      // Make a copy without the integrity field for verification
      const messageForVerification = JSON.parse(JSON.stringify(message));
      delete messageForVerification.security.integrity;
      
      const calculatedIntegrity = this.calculateIntegrity(messageForVerification);
      
      if (calculatedIntegrity !== originalIntegrity) {
        errors.push('Integrity verification failed');
      }
    }
    
    return {
      valid: errors.length === 0,
      errors,
      classification: security.classification || this.policy.default_level
    };
  }
  
  /**
   * Check if a flow is allowed based on classification and flow restrictions
   * 
   * @param classification Classification level
   * @param source Source of the flow
   * @param destination Destination of the flow
   * @returns Flow permission result
   */
  checkFlowPermission(
    classification: ClassificationLevel,
    source: string,
    destination: string
  ): {
    allowed: boolean;
    transformations?: string[];
  } {
    const levelConfig = this.policy.levels[classification];
    if (!levelConfig) {
      return { allowed: false };
    }
    
    // Find matching flow restriction
    for (const restriction of levelConfig.flow_restrictions) {
      // Check if source matches (using * as wildcard)
      const sourceMatch = restriction.source === '*' || restriction.source === source;
      
      // Check if destination matches (using * as wildcard)
      const destMatch = restriction.destination === '*' || restriction.destination === destination;
      
      if (sourceMatch && destMatch) {
        return {
          allowed: restriction.allowed,
          transformations: restriction.transformations
        };
      }
    }
    
    // No matching restriction, deny by default
    return { allowed: false };
  }
  
  /**
   * Apply transformations to a message based on flow restrictions
   * 
   * @param message MCP message
   * @param transformations List of transformations to apply
   * @returns Transformed message
   */
  applyTransformations(
    message: MCPMessageWithSecurity,
    transformations: string[]
  ): MCPMessageWithSecurity {
    // Return original if no transformations needed
    if (!transformations || transformations.length === 0) {
      return message;
    }
    
    // Create a deep copy of the message
    const transformedMessage = JSON.parse(JSON.stringify(message));
    
    // Initialize transformations array if not present
    if (!transformedMessage.security) {
      transformedMessage.security = { 
        classification: this.policy.default_level 
      };
    }
    
    if (!transformedMessage.security.transformations) {
      transformedMessage.security.transformations = [];
    }
    
    // Apply each transformation
    for (const transform of transformations) {
      switch (transform) {
        case 'redact_pii':
          // Apply PII redaction to params or result
          if (transformedMessage.params) {
            transformedMessage.params = this.redactPII(transformedMessage.params);
          }
          if (transformedMessage.result) {
            transformedMessage.result = this.redactPII(transformedMessage.result);
          }
          break;
          
        case 'redact':
          // Apply complete redaction to params or result
          if (transformedMessage.params) {
            transformedMessage.params = this.redact(transformedMessage.params);
          }
          if (transformedMessage.result) {
            transformedMessage.result = this.redact(transformedMessage.result);
          }
          break;
          
        case 'sanitize':
          // Apply sanitization to params or result
          if (transformedMessage.params) {
            transformedMessage.params = this.sanitize(transformedMessage.params);
          }
          if (transformedMessage.result) {
            transformedMessage.result = this.sanitize(transformedMessage.result);
          }
          break;
      }
      
      // Record the transformation
      transformedMessage.security.transformations.push(transform);
    }
    
    return transformedMessage;
  }
  
  /**
   * Redact PII information in content
   */
  private redactPII(content: any): any {
    if (typeof content === 'string') {
      // Redact common PII patterns
      return content
        .replace(/\b[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,}\b/gi, '[REDACTED EMAIL]')
        .replace(/\b(?:\d[ -]*?){13,16}\b/g, '[REDACTED CARD]')
        .replace(/\b\d{3}[-.]?\d{3}[-.]?\d{4}\b/g, '[REDACTED PHONE]')
        .replace(/\b\d{3}[-]?\d{2}[-]?\d{4}\b/g, '[REDACTED SSN]');
    } else if (typeof content === 'object' && content !== null) {
      const result: Record<string, any> = {};
      
      for (const [key, value] of Object.entries(content)) {
        // Redact sensitive field names
        if (['ssn', 'socialSecurity', 'creditCard', 'password', 'secret'].includes(key.toLowerCase())) {
          result[key] = '[REDACTED]';
        } else {
          result[key] = this.redactPII(value);
        }
      }
      
      return result;
    } else if (Array.isArray(content)) {
      return content.map(item => this.redactPII(item));
    } else {
      return content;
    }
  }
  
  /**
   * Completely redact content while preserving structure
   */
  private redact(content: any): any {
    if (typeof content === 'object' && content !== null && !Array.isArray(content)) {
      const result: Record<string, any> = {};
      
      for (const key of Object.keys(content)) {
        if (typeof content[key] === 'object' && content[key] !== null) {
          result[key] = this.redact(content[key]);
        } else {
          result[key] = '[REDACTED]';
        }
      }
      
      return result;
    } else if (Array.isArray(content)) {
      return content.map(() => '[REDACTED]');
    } else {
      return '[REDACTED]';
    }
  }
  
  /**
   * Sanitize potentially malicious content
   */
  private sanitize(content: any): any {
    if (typeof content === 'string') {
      // Basic sanitization
      return content
        .replace(/<script\b[^<]*(?:(?!<\/script>)<[^<]*)*<\/script>/gi, '')
        .replace(/<iframe\b[^<]*(?:(?!<\/iframe>)<[^<]*)*<\/iframe>/gi, '')
        .replace(/javascript:/gi, 'blocked:');
    } else if (typeof content === 'object' && content !== null) {
      const result: Record<string, any> = {};
      
      for (const [key, value] of Object.entries(content)) {
        result[key] = this.sanitize(value);
      }
      
      return result;
    } else if (Array.isArray(content)) {
      return content.map(item => this.sanitize(item));
    } else {
      return content;
    }
  }
  
  /**
   * Auto-classify a message based on content analysis
   */
  private autoClassifyMessage(message: MCPMessageWithSecurity): ClassificationLevel {
    // Convert message to string for analysis
    const content = JSON.stringify(message.params || message.result || {});
    
    // Simple pattern-based classification
    const patterns: Record<ClassificationLevel, RegExp[]> = {
      'RESTRICTED': [
        /\b(?:\d[ -]*?){13,16}\b/g, // Credit card numbers
        /\bpassword\b/i,
        /\bsecret\b/i,
        /\bprivate\skey\b/i
      ],
      'SENSITIVE': [
        /\b[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,}\b/gi, // Email
        /\b\d{3}[-.]?\d{3}[-.]?\d{4}\b/g, // Phone numbers
        /\b\d{3}[-]?\d{2}[-]?\d{4}\b/g, // SSN
        /\bsensitive\b/i,
        /\bconfidential\b/i
      ],
      'INTERNAL': [
        /\binternal\b/i,
        /\bemployee\b/i,
        /\bcompany\b/i
      ],
      'PUBLIC': []
    };
    
    // Check patterns from most restrictive to least
    const levels: ClassificationLevel[] = ['RESTRICTED', 'SENSITIVE', 'INTERNAL', 'PUBLIC'];
    
    for (const level of levels) {
      const levelPatterns = patterns[level];
      
      for (const pattern of levelPatterns) {
        if (pattern.test(content)) {
          return level;
        }
      }
    }
    
    // Default classification
    return this.policy.default_level;
  }
  
  /**
   * Calculate integrity hash for a message
   */
  private calculateIntegrity(message: MCPMessageWithSecurity): string {
    // In a real implementation, this would use a proper HMAC
    // This is just a simplified example
    const content = JSON.stringify(message);
    
    // Simple hash function (NOT for production use!)
    let hash = 0;
    for (let i = 0; i < content.length; i++) {
      const char = content.charCodeAt(i);
      hash = ((hash << 5) - hash) + char;
      hash = hash & hash; // Convert to 32bit integer
    }
    
    return `sha256:${Math.abs(hash).toString(16).padStart(8, '0')}`;
  }
}

/**
 * Example usage of Security Annotations
 */
export function demonstrateSecurityAnnotations(): void {
  // Define classification policy
  const policy: ClassificationPolicy = {
    levels: {
      'PUBLIC': {
        description: 'Information safe for public disclosure',
        flow_restrictions: [
          { source: '*', destination: '*', allowed: true }
        ],
        required_capability: 'none',
        audit_level: 'metadata'
      },
      'INTERNAL': {
        description: 'Information restricted to the application',
        flow_restrictions: [
          { source: 'mcp_client', destination: 'mcp_server', allowed: true },
          { source: 'mcp_server', destination: 'mcp_client', allowed: true },
          { source: '*', destination: 'external', allowed: false }
        ],
        required_capability: 'basic_access',
        audit_level: 'metadata'
      },
      'SENSITIVE': {
        description: 'Information requiring protection',
        flow_restrictions: [
          { source: 'mcp_client', destination: 'mcp_server', allowed: true },
          { 
            source: 'mcp_server', 
            destination: 'mcp_client', 
            allowed: true, 
            transformations: ['redact_pii'] 
          },
          { source: '*', destination: 'external', allowed: false }
        ],
        required_capability: 'sensitive_access',
        audit_level: 'full'
      },
      'RESTRICTED': {
        description: 'Highly sensitive information',
        flow_restrictions: [
          { 
            source: 'mcp_client', 
            destination: 'mcp_server', 
            allowed: true, 
            transformations: ['sanitize'] 
          },
          { 
            source: 'mcp_server', 
            destination: 'mcp_client', 
            allowed: true, 
            transformations: ['redact'] 
          },
          { source: '*', destination: 'external', allowed: false }
        ],
        required_capability: 'restricted_access',
        audit_level: 'full'
      }
    },
    default_level: 'INTERNAL',
    auto_classification: true
  };
  
  // Create security managers for client and server
  const clientManager = new SecurityAnnotationManager(
    policy,
    'mcp_client',
    'client-key'
  );
  
  const serverManager = new SecurityAnnotationManager(
    policy,
    'mcp_server',
    'server-key'
  );
  
  // Client creates a request
  const request: MCPMessageWithSecurity = {
    jsonrpc: '2.0',
    method: 'getUserProfile',
    params: {
      userId: '12345',
      includePersonalInfo: true
    },
    id: 1
  };
  
  // Client adds security annotations
  const secureRequest = clientManager.addSecurityAnnotations(request);
  console.log('Secure Request:', JSON.stringify(secureRequest, null, 2));
  
  // Server verifies request security
  const requestVerification = serverManager.verifySecurityAnnotations(secureRequest);
  console.log('Request Verification:', requestVerification);
  
  // Check flow permission
  const requestFlow = serverManager.checkFlowPermission(
    secureRequest.security!.classification,
    secureRequest.security!.source!,
    'mcp_server'
  );
  console.log('Request Flow Permission:', requestFlow);
  
  // Apply any required transformations
  const transformedRequest = serverManager.applyTransformations(
    secureRequest,
    requestFlow.transformations || []
  );
  
  // Server creates a response with sensitive data
  const response: MCPMessageWithSecurity = {
    jsonrpc: '2.0',
    result: {
      userId: '12345',
      name: 'John Doe',
      email: 'john.doe@example.com',
      ssn: '123-45-6789',
      address: '123 Main St'
    },
    id: 1
  };
  
  // Server adds security annotations
  const secureResponse = serverManager.addSecurityAnnotations(
    response,
    'SENSITIVE' // Explicitly set classification
  );
  console.log('Secure Response:', JSON.stringify(secureResponse, null, 2));
  
  // Check flow permission for response
  const responseFlow = serverManager.checkFlowPermission(
    secureResponse.security!.classification,
    secureResponse.security!.source!,
    'mcp_client'
  );
  console.log('Response Flow Permission:', responseFlow);
  
  // Apply any required transformations
  const transformedResponse = serverManager.applyTransformations(
    secureResponse,
    responseFlow.transformations || []
  );
  console.log('Transformed Response:', JSON.stringify(transformedResponse, null, 2));
  
  // Client verifies response security
  const responseVerification = clientManager.verifySecurityAnnotations(transformedResponse);
  console.log('Response Verification:', responseVerification);
}

// Example integration with MCP client/server

/**
 * Integrate Security Annotations with an MCP client
 * 
 * @param mcpClient MCP client instance
 * @param policy Classification policy
 * @param sourceId Source identifier
 * @param integrityKey Integrity protection key
 * @returns Enhanced MCP client with security annotations
 */
export function enhanceMCPClientWithSecurityAnnotations(
  mcpClient: any,
  policy: ClassificationPolicy,
  sourceId: string,
  integrityKey?: string
): any {
  // Create security annotation manager
  const securityManager = new SecurityAnnotationManager(
    policy,
    sourceId,
    integrityKey
  );
  
  // Keep track of sequence numbers from servers
  const serverSequences: Record<string, number> = {};
  
  // Wrap the sendRequest method to add security annotations
  const originalSendRequest = mcpClient.sendRequest;
  mcpClient.sendRequest = async function(method: string, params: any, id?: any) {
    const request: MCPMessageWithSecurity = {
      jsonrpc: '2.0',
      method,
      params,
      id: id || Math.floor(Math.random() * 10000)
    };
    
    // Add security annotations
    const secureRequest = securityManager.addSecurityAnnotations(request);
    
    // Send the secure request
    const response = await originalSendRequest.call(this, method, params, secureRequest.id);
    
    // Verify response security
    if (response && response.security) {
      const source = response.security.source || '';
      const lastSequence = serverSequences[source] || 0;
      
      const verification = securityManager.verifySecurityAnnotations(
        response,
        undefined, // Don't verify specific source
        lastSequence
      );
      
      if (!verification.valid) {
        throw new Error(`Security verification failed: ${verification.errors.join(', ')}`);
      }
      
      // Update last sequence number
      if (response.security.sequence !== undefined) {
        serverSequences[source] = response.security.sequence;
      }
      
      // Check flow permission
      const flowPermission = securityManager.checkFlowPermission(
        verification.classification,
        response.security.source || '',
        sourceId
      );
      
      if (!flowPermission.allowed) {
        throw new Error(`Flow not allowed for classification: ${verification.classification}`);
      }
    }
    
    return response;
  };
  
  // Return the enhanced client
  return mcpClient;
}
```

 

















 
### 1.1 Technical Specification

### 1.2 Implementation Roadmap

1. **Weeks 1-2: Architecture and Design**
   - Complete ESM architecture specification
   - Define security module interfaces
   - Establish protocol-level security annotation standards
   - Design plugin management system
   - Create test strategy document

2. **Weeks 3-6: Core Implementation**
   - Develop ESM reference implementation
   - Implement foundational security modules:
     - Validation Module
     - Classification Module
     - Transformation Module
     - Transport Security Module
   - Create protocol-level security annotation utilities
   - Build integration adapters for MCP client/server SDK

3. **Weeks 7-10: Testing and Refinement**
   - Develop comprehensive test suite
   - Conduct security penetration testing
   - Refine implementation based on test results
   - Create integration examples
   - Develop performance benchmarks

4. **Weeks 11-12: Documentation and Release**
   - Finalize developer documentation
   - Create integration guides
   - Prepare sample configurations
   - Publish initial release
   - Establish feedback channels

### 1.3 Resource Requirements

- **Development Team**:
  - 2 Senior Security Engineers
  - 1 MCP Protocol Expert
  - 1 Test Engineer
  - 1 Technical Writer

- **Infrastructure**:
  - Development environment with CI/CD pipeline
  - Test environment for security validation
  - Documentation hosting platform
  - Community discussion forums

- **External Dependencies**:
  - Access to MCP reference implementations
  - Security testing tools
  - Performance benchmarking suite

## 2. Protocol-Level Security Annotations

### 2.1 Standards Document

The Protocol-Level Security Annotations standard defines how security metadata is embedded directly within MCP messages. This approach ensures security properties travel with the messages themselves, providing:

1. **Classification**: Sensitivity level of message content
2. **Integrity**: Verification of message integrity
3. **Source**: Identification of the sender
4. **Sequence**: Protection against replay attacks
5. **Transformations**: Record of security-related modifications

The standard establishes:
- Required and optional security fields
- Field formats and validation rules
- Default security behaviors
- Integration patterns with MCP

### 2.2 Classification System

GUARDRAIL uses a four-tier classification system for data:

1. **PUBLIC**: Information safe for public disclosure
2. **INTERNAL**: Information restricted to the application
3. **SENSITIVE**: Information requiring additional protection
4. **RESTRICTED**: Highly sensitive information with strict controls

Each level has associated:
- Flow control policies
- Transformation requirements
- Capability requirements
- Audit logging requirements

### 2.3 Implementation Guide

# GUARDRAIL Integration Guide
## Phase 1: Foundation Implementation

This guide provides step-by-step instructions for integrating the GUARDRAIL Phase 1 security components into your MCP (Model Context Protocol) implementation. Phase 1 focuses on establishing the foundational building blocks: Extensible Security Middleware (ESM) and Protocol-Level Security Annotations.

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Installation](#installation)
3. [Integrating ESM with MCP Client](#integrating-esm-with-mcp-client)
4. [Integrating ESM with MCP Server](#integrating-esm-with-mcp-server)
5. [Configuring Security Modules](#configuring-security-modules)
6. [Working with Protocol-Level Security Annotations](#working-with-protocol-level-security-annotations)
7. [Testing Your Integration](#testing-your-integration)
8. [Troubleshooting](#troubleshooting)
9. [Next Steps](#next-steps)

## Prerequisites

Before you begin, ensure you have:

- MCP client/server SDK (version 1.0 or later)
- Node.js (version 16 or later) for JavaScript/TypeScript implementations
- Python (version 3.8 or later) for Python implementations
- Basic understanding of your application's security requirements
- Development environment with appropriate permissions

## Installation

### Using npm (JavaScript/TypeScript)

```bash
npm install @guardrail/esm @guardrail/security-annotations
```

### Using pip (Python)

```bash
pip install guardrail-esm guardrail-security-annotations
```

## Integrating ESM with MCP Client

### JavaScript/TypeScript

```typescript
import { Client } from '@modelcontextprotocol/sdk/client';
import { ExtensibleSecurityMiddleware } from '@guardrail/esm';
import { ClassificationModule, ValidationModule, TransformationModule } from '@guardrail/esm/modules';

// Create a standard MCP client
const mcpClient = new Client({
  name: 'example-client',
  version: '1.0.0'
});

// Create ESM instance
const esm = new ExtensibleSecurityMiddleware({
  security: {
    initialTrustScore: 0.8,
    initialThreatLevel: 'low',
    capabilities: ['basic_access']
  },
  pipeline: {
    modules: {
      // Configuration for built-in modules
    },
    validationFailureAction: 'block',
    continueOnError: false
  }
});

// Register required modules
esm.registerModule(new ValidationModule(), {
  enabled: true,
  order: 10,
  options: { strictMode: true, maxSizeBytes: 1048576 }
});

esm.registerModule(new ClassificationModule(), {
  enabled: true,
  order: 20,
  options: { defaultClassification: 'INTERNAL' }
});

esm.registerModule(new TransformationModule(), {
  enabled: true,
  order: 30,
  options: { transformations: ['sanitize', 'redact_pii'] }
});

// Intercept outbound messages
mcpClient.setOutboundInterceptor(async (message) => {
  return await esm.processOutbound(message);
});

// Intercept inbound messages
mcpClient.setInboundInterceptor(async (message) => {
  return await esm.processInbound(message);
});

export default mcpClient;
```

### Python

```python
from mcp.client import Client
from guardrail.esm import ExtensibleSecurityMiddleware
from guardrail.esm.modules import ValidationModule, ClassificationModule, TransformationModule

# Create a standard MCP client
mcp_client = Client(
    name="example-client",
    version="1.0.0"
)

# Create ESM instance
esm = ExtensibleSecurityMiddleware(
    security={
        "initial_trust_score": 0.8,
        "initial_threat_level": "low",
        "capabilities": ["basic_access"]
    },
    pipeline={
        "modules": {},
        "validation_failure_action": "block",
        "continue_on_error": False
    }
)

# Register required modules
esm.register_module(ValidationModule(), {
    "enabled": True,
    "order": 10,
    "options": {
        "strict_mode": True,
        "max_size_bytes": 1048576
    }
})

esm.register_module(ClassificationModule(), {
    "enabled": True,
    "order": 20,
    "options": {
        "default_classification": "INTERNAL"
    }
})

esm.register_module(TransformationModule(), {
    "enabled": True,
    "order": 30,
    "options": {
        "transformations": ["sanitize", "redact_pii"]
    }
})

# Intercept outbound messages
def outbound_interceptor(message):
    return esm.process_outbound(message)

# Intercept inbound messages
def inbound_interceptor(message):
    return esm.process_inbound(message)

mcp_client.set_outbound_interceptor(outbound_interceptor)
mcp_client.set_inbound_interceptor(inbound_interceptor)
```

## Integrating ESM with MCP Server

### JavaScript/TypeScript

```typescript
import { Server } from '@modelcontextprotocol/sdk/server';
import { ExtensibleSecurityMiddleware } from '@guardrail/esm';
import { ValidationModule, ClassificationModule, TransformationModule } from '@guardrail/esm/modules';

// Create a standard MCP server
const mcpServer = new Server({
  name: 'example-server',
  version: '1.0.0'
}, {
  capabilities: {
    resources: {}
  }
});

// Create ESM instance
const esm = new ExtensibleSecurityMiddleware({
  security: {
    initialTrustScore: 0.8,
    initialThreatLevel: 'low',
    capabilities: ['basic_access']
  },
  pipeline: {
    modules: {
      // Configuration for built-in modules
    },
    validationFailureAction: 'block',
    continueOnError: false
  }
});

// Register security modules
esm.registerModule(new ValidationModule(), {
  enabled: true,
  order: 10,
  options: { strictMode: true, maxSizeBytes: 1048576 }
});

esm.registerModule(new ClassificationModule(), {
  enabled: true,
  order: 20,
  options: { defaultClassification: 'INTERNAL' }
});

esm.registerModule(new TransformationModule(), {
  enabled: true,
  order: 30,
  options: { transformations: ['sanitize', 'redact_pii'] }
});

// Intercept request handling
const originalHandleRequest = mcpServer.handleRequest;
mcpServer.handleRequest = async function(request) {
  // Process inbound request through ESM
  const secureRequest = await esm.processInbound(request);
  
  // Process request
  const response = await originalHandleRequest.call(this, secureRequest);
  
  // Process outbound response through ESM
  return await esm.processOutbound(response);
};

export default mcpServer;
```

### Python

```python
from mcp.server import Server
from guardrail.esm import ExtensibleSecurityMiddleware
from guardrail.esm.modules import ValidationModule, ClassificationModule, TransformationModule

# Create a standard MCP server
mcp_server = Server(
    name="example-server",
    version="1.0.0",
    capabilities={
        "resources": {}
    }
)

# Create ESM instance
esm = ExtensibleSecurityMiddleware(
    security={
        "initial_trust_score": 0.8,
        "initial_threat_level": "low",
        "capabilities": ["basic_access"]
    },
    pipeline={
        "modules": {},
        "validation_failure_action": "block",
        "continue_on_error": False
    }
)

# Register security modules
esm.register_module(ValidationModule(), {
    "enabled": True,
    "order": 10,
    "options": {
        "strict_mode": True,
        "max_size_bytes": 1048576
    }
})

esm.register_module(ClassificationModule(), {
    "enabled": True,
    "order": 20,
    "options": {
        "default_classification": "INTERNAL"
    }
})

esm.register_module(TransformationModule(), {
    "enabled": True,
    "order": 30,
    "options": {
        "transformations": ["sanitize", "redact_pii"]
    }
})

# Save original handler
original_handle_request = mcp_server.handle_request

# Replace with secure handler
async def secure_handle_request(request):
    # Process inbound request through ESM
    secure_request = await esm.process_inbound(request)
    
    # Process request
    response = await original_handle_request(secure_request)
    
    # Process outbound response through ESM
    return await esm.process_outbound(response)

# Apply secure handler
mcp_server.handle_request = secure_handle_request
```

## Configuring Security Modules

### Classification Module

The Classification Module analyzes message content and assigns appropriate sensitivity classifications. Configuration options include:

```javascript
{
  "defaultClassification": "INTERNAL", // Default level if no patterns match
  "rules": [
    {
      "id": "personal-data",
      "name": "Personal Data",
      "patterns": [
        { "type": "regex", "value": "\\b[A-Z0-9._%+-]+@[A-Z0-9.-]+\\.[A-Z]{2,}\\b", "weight": 0.7 },
        { "type": "regex", "value": "\\b\\d{3}-\\d{2}-\\d{4}\\b", "weight": 0.9 },
        { "type": "semantic", "value": "personal", "weight": 0.5 }
      ],
      "threshold": 0.7,
      "classification": "SENSITIVE"
    },
    {
      "id": "financial-data",
      "name": "Financial Data",
      "patterns": [
        { "type": "regex", "value": "\\b(?:\\d[ -]*?){13,16}\\b", "weight": 0.8 },
        { "type": "semantic", "value": "financial", "weight": 0.6 }
      ],
      "threshold": 0.6,
      "classification": "RESTRICTED"
    }
  ]
}
```

### Validation Module

The Validation Module verifies message structure and content against security policies:

```javascript
{
  "strictMode": true,        // Requires security field to be present
  "maxSizeBytes": 1048576,   // Maximum message size (1MB)
  "requireJSONRPC": true,    // Enforce JSON-RPC 2.0 format
  "validateSequence": true,  // Check for proper sequence numbers
  "injectionPatterns": [     // Custom injection patterns to detect
    "/\\${.+}/i",
    "/<script\\b[^<]*(?:(?!<\\/script>)<[^<]*)*<\\/script>/i",
    "/\\b(exec|eval|function\\(|setTimeout|setInterval)\\(/i",
    "/\\b(SELECT|INSERT|UPDATE|DELETE|DROP|UNION|ALTER)\\b/i"
  ]
}
```

### Transformation Module

The Transformation Module applies security transformations to messages:

```javascript
{
  "transformations": ["sanitize", "redact_pii", "redact"],  // Available transformations
  "transformationMap": {                                    // When to apply which transformation
    "PUBLIC": [],
    "INTERNAL": ["sanitize"],
    "SENSITIVE": ["sanitize", "redact_pii"],
    "RESTRICTED": ["sanitize", "redact"]
  },
  "sensitiveFields": ["password", "ssn", "creditCard", "secret", "token"]  // Fields to always redact
}
```

## Working with Protocol-Level Security Annotations

### Message Structure

When using security annotations, MCP messages include a `security` field:

```json
{
  "jsonrpc": "2.0",
  "method": "getResource",
  "params": {
    "resourceId": "doc123",
    "includeMetadata": true
  },
  "security": {
    "classification": "INTERNAL",
    "integrity": "sha256:8a7b3c2d1e...",
    "source": "client:app123",
    "sequence": 15
  },
  "id": 1
}
```

### Security Field Components

- **classification**: Sensitivity level ("PUBLIC", "INTERNAL", "SENSITIVE", "RESTRICTED")
- **integrity**: Cryptographic hash to verify message integrity
- **source**: Identifier of the sender
- **sequence**: Monotonically increasing counter to prevent replay attacks
- **transformations**: Array of transformations applied to the message (optional)

### Manual Security Annotation

For specialized cases where you need more control:

```typescript
import { SecurityAnnotationManager, ClassificationPolicy } from '@guardrail/security-annotations';

// Define policy
const policy: ClassificationPolicy = {
  levels: {
    'PUBLIC': {
      description: 'Information safe for public disclosure',
      flow_restrictions: [
        { source: '*', destination: '*', allowed: true }
      ],
      required_capability: 'none',
      audit_level: 'metadata'
    },
    // Add other classification levels...
  },
  default_level: 'INTERNAL',
  auto_classification: true
};

// Create manager
const annotationManager = new SecurityAnnotationManager(
  policy,
  'client-id',
  'integrity-key'
);

// Add annotations to a message
const secureMessage = annotationManager.addSecurityAnnotations(message, 'SENSITIVE');

// Verify annotations on a received message
const verification = annotationManager.verifySecurityAnnotations(receivedMessage);
if (!verification.valid) {
  // Handle security violation
  console.error(`Security verification failed: ${verification.errors.join(', ')}`);
}
```

## Testing Your Integration

### Basic Security Flow Test

```typescript
async function testSecurityFlow() {
  // Create a secure client
  const client = createSecureClient();
  
  // Connect to server
  await client.connect(transport);
  
  try {
    // Try a standard request
    const response = await client.sendRequest('getPublicResource', { id: 'public1' });
    console.log('Standard request successful:', response);
    
    // Try a request with sensitive data
    const sensitiveResponse = await client.sendRequest('getUserData', { 
      userId: '12345',
      includePersonalInfo: true 
    });
    console.log('Sensitive request successful:', sensitiveResponse);
    
    // Try a request with potentially malicious content
    try {
      const maliciousResponse = await client.sendRequest('searchData', { 
        query: '<script>alert("XSS");</script>' 
      });
      console.log('Malicious request response:', maliciousResponse);
    } catch (error) {
      console.log('Malicious request correctly blocked:', error.message);
    }
  } catch (error) {
    console.error('Test failed:', error);
  }
}
```

### Validating Security Annotations

```typescript
function validateSecurityAnnotations(message) {
  // Check if security field exists
  if (!message.security) {
    return { valid: false, error: 'Missing security annotations' };
  }
  
  // Check classification
  if (!['PUBLIC', 'INTERNAL', 'SENSITIVE', 'RESTRICTED'].includes(message.security.classification)) {
    return { valid: false, error: 'Invalid classification' };
  }
  
  // Check source
  if (!message.security.source) {
    return { valid: false, error: 'Missing source identifier' };
  }
  
  // Check sequence
  if (typeof message.security.sequence !== 'number') {
    return { valid: false, error: 'Invalid sequence number' };
  }
  
  return { valid: true };
}
```

## Troubleshooting

### Common Issues

#### Missing Security Field Error

**Problem**: Receiving `Security field is required in strict mode` errors.

**Solution**: Either add security annotations to your messages or configure the ValidationModule with `strictMode: false` for a gradual adoption approach.

```typescript
esm.registerModule(new ValidationModule(), {
  enabled: true,
  order: 10,
  options: { strictMode: false, maxSizeBytes: 1048576 }
});
```

#### Classification Too Restrictive

**Problem**: Messages being classified as too sensitive, restricting necessary flows.

**Solution**: Adjust classification rules or manually override classifications for specific operations:

```typescript
// Override classification for specific operations
if (message.method === 'getPublicResource') {
  message.security = {
    ...message.security,
    classification: 'PUBLIC'
  };
}
```

#### Performance Concerns

**Problem**: Security processing adding noticeable latency.

**Solution**: 
1. Optimize module ordering (put faster modules first)
2. Disable unused modules
3. Configure modules for lighter processing
4. Consider using the embedded deployment model

```typescript
// Performance-optimized configuration
esm.registerModule(new ValidationModule(), {
  enabled: true,
  order: 10,
  options: { 
    strictMode: false,
    maxSizeBytes: 1048576,
    skipDetailedChecks: true  // Faster but less thorough
  }
});
```

## Next Steps

After successfully implementing GUARDRAIL Phase 1 components, consider:

1. **Monitoring and Metrics**: Add monitoring to track security events and performance impact
2. **Custom Security Modules**: Develop modules specific to your application's security needs
3. **Advanced Configuration**: Fine-tune classification rules and transformation policies
4. **Preparing for Phase 2**: Explore upcoming GUARDRAIL features like the Context Verification Layer

For more information, visit the [GUARDRAIL Documentation Portal](https://guardrail-project.github.io/docs/) or join our [Community Discussion Forum](https://github.com/guardrail-project/discussions).

## 3. Documentation and Community Infrastructure

### 3.1 Documentation Portal

# GUARDRAIL Documentation Portal Structure

The GUARDRAIL Documentation Portal will serve as the central resource for all documentation, guides, and reference materials for the GUARDRAIL security framework. This document outlines the structure, content plans, and implementation strategy for the documentation portal.

## Portal Sections and Content Plan

### 1. Home Page

**Purpose:** Provide an overview of GUARDRAIL and help users quickly find the information they need.

**Content:**
- Brief introduction to GUARDRAIL
- Key features and benefits
- Getting started links
- Latest news and updates
- Quick navigation to most common resources

### 2. Getting Started

**Purpose:** Help new users understand and start using GUARDRAIL quickly.

**Content:**
- Introduction to GUARDRAIL concepts
- Installation guides for different environments
- Basic integration tutorials
- Simple examples with code samples
- First-time setup checklist

**Subsections:**
- **Quick Start Guide**
  - 15-minute guide to implement basic GUARDRAIL security
  - Minimal implementation examples

- **Installation**
  - Environment-specific installation instructions
  - Dependency requirements
  - Verification steps

- **Basic Concepts**
  - Security layers explanation
  - Core terminology
  - Architecture diagrams

### 3. Core Components

**Purpose:** Provide detailed information about each GUARDRAIL component.

**Content:**
- Extensible Security Middleware (ESM)
  - Architecture
  - Module system
  - Integration points
  - Configuration options

- Protocol-Level Security Annotations
  - Message structure
  - Classification system
  - Integrity mechanisms
  - Flow control integration

- Additional components (added in future phases)

**Subsections:**
- **Component Guides**
  - Detailed documentation for each component
  - Configuration examples
  - Best practices

- **API Reference**
  - Complete API documentation
  - Method signatures
  - Parameter descriptions
  - Return values

### 4. Implementation Guides

**Purpose:** Provide practical guidance for implementing GUARDRAIL in various scenarios.

**Content:**
- Step-by-step integration examples
- Environment-specific considerations
- Deployment model comparisons
- Configuration templates

**Subsections:**
- **Basic Implementation**
  - Minimal security setup
  - Essential configurations

- **Advanced Implementation**
  - Comprehensive security integration
  - Custom module development
  - Performance optimization

- **Deployment Models**
  - Embedded deployment guide
  - Gateway deployment guide
  - Service mesh deployment guide (future)

### 5. Security Policies

**Purpose:** Guide users in developing and implementing effective security policies.

**Content:**
- Policy structure and format
- Classification policy templates
- Flow control policy examples
- Transformation rules

**Subsections:**
- **Classification Policies**
  - Standard classification levels
  - Custom classification systems
  - Classification rule development

- **Flow Control Policies**
  - Defining flow restrictions
  - Flow permission models
  - Cross-boundary policies

- **Transformation Policies**
  - Content transformation techniques
  - Redaction patterns
  - Sanitization configurations

### 6. API Reference

**Purpose:** Provide comprehensive technical reference for all APIs.

**Content:**
- Complete API documentation
- Interface definitions
- Type specifications
- Error codes and handling

**Subsections:**
- **Core APIs**
  - ESM APIs
  - Security Annotation APIs
  - Configuration APIs

- **Module APIs**
  - Built-in module interfaces
  - Custom module development
  - Module lifecycle hooks

- **Integration APIs**
  - MCP integration points
  - Transport layer hooks
  - External system connectors

### 7. Tutorials

**Purpose:** Offer step-by-step guides for common tasks and scenarios.

**Content:**
- Task-oriented guides
- Scenario-based examples
- Problem-solving walkthroughs

**Subsections:**
- **Implementation Tutorials**
  - Basic security setup
  - Custom classification rules
  - Flow control implementation

- **Development Tutorials**
  - Custom module development
  - Extending built-in modules
  - Integration with external systems

- **Operational Tutorials**
  - Monitoring and alerting
  - Incident response
  - Performance optimization

### 8. Samples & Examples

**Purpose:** Provide ready-to-use code samples and application examples.

**Content:**
- Code snippets for common tasks
- Sample applications
- Configuration examples

**Subsections:**
- **Code Samples**
  - Basic integration examples
  - Module configuration samples
  - Policy definition examples

- **Sample Applications**
  - Simple MCP client with GUARDRAIL
  - Simple MCP server with GUARDRAIL
  - Complete application examples

- **Configuration Examples**
  - Basic security configurations
  - Advanced security profiles
  - Environment-specific settings

### 9. Community

**Purpose:** Foster community engagement and collaboration.

**Content:**
- Community guidelines
- Discussion forums
- Contribution guide
- Issue reporting process

**Subsections:**
- **Contributing Guide**
  - Code contribution workflow
  - Documentation contribution process
  - Testing requirements

- **Community Resources**
  - Discussion forums
  - Chat channels
  - Community meetups

- **Support**
  - Issue tracking
  - Support channels
  - Commercial support options

### 10. FAQs & Troubleshooting

**Purpose:** Help users resolve common issues.

**Content:**
- Frequently asked questions
- Common error solutions
- Troubleshooting guides

**Subsections:**
- **Installation Issues**
  - Dependency problems
  - Environment-specific issues
  - Verification failures

- **Integration Problems**
  - MCP compatibility issues
  - Message processing errors
  - Performance problems

- **Security Configuration**
  - Classification issues
  - Flow control problems
  - Transformation errors

## Documentation Infrastructure

### Technology Stack

- **Documentation Framework:** Docusaurus
- **Hosting Platform:** GitHub Pages
- **Source Repository:** GitHub
- **Search:** Algolia DocSearch
- **API Documentation:** TypeDoc (TypeScript) and Sphinx (Python)
- **Code Examples:** CodeSandbox integration

### Versioning Strategy

- Documentation will be versioned to match GUARDRAIL releases
- Each major version will have its own documentation branch
- Latest version will be the default view
- Version selector will allow viewing older documentation

### Continuous Integration/Continuous Deployment

- Documentation builds triggered by commits to main branch
- Pull request previews for documentation changes
- Automated link checking and validation
- Style and formatting enforcement

### Content Management Process

1. Documentation issues and requests tracked in GitHub
2. Documentation changes follow the same PR process as code
3. Technical reviews required for all documentation changes
4. Regular documentation audits and refreshes scheduled

## Implementation Timeline

### Phase 1 (Weeks 1-4)

- Set up documentation infrastructure
- Create basic structure and navigation
- Develop home page and getting started content
- Establish style guide and templates

### Phase 2 (Weeks 5-8)

- Develop core component documentation
- Create API reference documentation
- Implement initial tutorials
- Add basic code samples

### Phase 3 (Weeks 9-12)

- Develop advanced implementation guides
- Add deployment model documentation
- Expand tutorials and examples
- Create troubleshooting guides and FAQs

### Phase 4 (Ongoing)

- Regular content updates
- Community-contributed examples
- Feedback-driven improvements
- New feature documentation

## Documentation Maintenance

### Roles and Responsibilities

- **Documentation Lead:** Oversees documentation strategy and quality
- **Content Contributors:** Develop technical content and examples
- **Technical Reviewers:** Ensure technical accuracy
- **Community Moderators:** Manage community contributions

### Update Process

- Regular documentation review cycles
- Issue-based documentation improvements
- Documentation updates coordinated with code releases
- Community feedback collection and incorporation

### Quality Control

- Technical accuracy reviews
- Clarity and usability testing
- Link and reference validation
- Code example verification

## Content Style Guidelines

### Writing Style

- Clear, concise, and direct language
- Active voice and present tense
- Second person (you/your) for instructions
- Task-oriented approach for procedures

### Structure

- Consistent heading hierarchy
- Brief introductions for each section
- Logical progression of concepts
- Summary conclusions where appropriate

### Code Examples

- Consistent formatting with syntax highlighting
- Complete, working examples
- Clear comments explaining key points
- Multiple language versions where appropriate

### Visual Elements

- Relevant diagrams for complex concepts
- Consistent visual style
- Alt text for accessibility
- Appropriate use of callouts and notes

### 3.2 Community Governance Structure


# GUARDRAIL Community Governance Framework

## Introduction

The GUARDRAIL Community Governance Framework establishes the structure, processes, and principles that guide the development, maintenance, and evolution of the GUARDRAIL security framework. This governance model aims to ensure the project remains sustainable, technically excellent, and responsive to security needs while maintaining its open and collaborative nature.

## Vision and Principles

### Vision

GUARDRAIL will be the industry standard for securing LLM applications, providing robust, adaptable security controls that protect against evolving threats while maintaining compatibility with the Model Context Protocol (MCP) ecosystem.

### Guiding Principles

1. **Security First**: Security effectiveness is our primary concern in all decisions
2. **Practical Adoption**: Design for incremental adoption and real-world implementation
3. **Open Governance**: Transparent decision-making and inclusive participation
4. **Technical Excellence**: Rigorous standards for code quality and security effectiveness
5. **Sustainability**: Long-term project viability through stable governance and contribution processes
6. **Interoperability**: Maintain compatibility with evolving MCP standards
7. **Community Driven**: Responsive to community needs and contributions

## Governance Structure

GUARDRAIL employs a multi-tiered governance model designed to balance efficiency, expertise, quality, and community representation:

### 1. Technical Steering Committee (TSC)

**Role**: Provides technical leadership, sets architectural direction, and approves major design decisions.

**Responsibilities**:
- Approve architectural changes and major feature additions
- Review and establish technical standards
- Resolve technical disputes
- Manage the roadmap and release planning
- Oversee security review process

**Composition**:
- 5-7 members with deep security and/or LLM expertise
- Mix of founding contributors and elected community members
- Terms of 1 year, staggered to ensure continuity
- Chair elected by TSC members annually

**Decision Process**:
- Regular meetings (bi-weekly)
- Decisions require majority approval
- Public meeting notes and decision records
- Significant decisions require a documented proposal (GRIP - GUARDRAIL Improvement Proposal)

### 2. Working Groups

**Role**: Focus on specific aspects of the project with delegated authority from the TSC.

**Core Working Groups**:
- **Security Layer WG**: Focuses on security layer implementations and enhancements
- **Integration WG**: Focuses on MCP integration and interoperability
- **Documentation WG**: Manages documentation and learning resources
- **Community WG**: Manages outreach, events, and communications

**Structure**:
- Each working group has a chair appointed by the TSC
- Working groups establish their own meeting schedules and processes
- Working groups report to the TSC regularly
- Any community member can join a working group

### 3. Maintainers

**Role**: Day-to-day technical leadership of specific components or modules.

**Responsibilities**:
- Review and merge pull requests
- Triage and address issues
- Ensure code quality and test coverage
- Mentor contributors
- Implement TSC technical decisions

**Selection**:
- Appointed by the TSC based on consistent quality contributions
- Component-specific maintainers with defined areas of responsibility
- No fixed term, based on active participation

### 4. Contributors

**Role**: Anyone who contributes to the project in any capacity.

**Types of Contributors**:
- Code contributors
- Documentation contributors
- Testing contributors
- Design contributors
- Community supporters

**Contributor Path**:
- Clear progression from first-time contributor to maintainer status
- Contribution guidelines and mentoring support
- Recognition program for significant contributions

## Decision-Making Process

GUARDRAIL uses a structured decision-making process based on the level of impact:

### 1. Minor Changes (Day-to-Day Decisions)

- Handled directly by maintainers
- Regular pull request process with maintainer approval
- Must pass CI/CD checks and code review
- No formal proposal needed

### 2. Significant Changes (Component-Level Decisions)

- Requires discussion in relevant working group
- Simple proposal document outlining the change
- Consensus of component maintainers required
- Working group chair can resolve disputes

### 3. Major Changes (Project-Level Decisions)

- Requires formal GUARDRAIL Improvement Proposal (GRIP)
- Public comment period of at least two weeks
- Review and approval by the TSC
- May require security review
- Public announcement of decisions

### GUARDRAIL Improvement Proposal (GRIP) Process

1. **Idea Stage**: Initial concept discussion in community forums
2. **Draft Proposal**: Author creates detailed proposal following template
3. **Review Stage**: Public review and discussion period
4. **TSC Vote**: Formal consideration by the Technical Steering Committee
5. **Implementation**: Approved proposals move to implementation
6. **Retrospective**: Review of implementation outcomes

## Contribution Process

### Code Contributions

1. **Issue Creation**: All changes start with an issue (bug, feature, etc.)
2. **Discussion**: Initial discussion on approach and design
3. **Implementation**: Work performed in a feature branch
4. **Pull Request**: Submission including tests and documentation
5. **Review**: Maintainer code review and feedback
6. **CI/CD**: Automated tests and validation
7. **Merge**: Approved changes merged to main branch

### Review Requirements

- All code requires at least one maintainer approval
- Security-critical code requires two approvals
- All tests must pass
- Documentation must be updated
- Security review for sensitive components

### Release Process

- Time-based release schedule (every 2 months)
- Semantic versioning (MAJOR.MINOR.PATCH)
- Release candidates for testing
- Release notes detailing changes and impacts
- Security patches may receive out-of-cycle releases

## Community Participation

### Communication Channels

- **GitHub**: Primary repository and issue tracking
- **Discussion Forums**: Long-form discussions and proposals
- **Chat Platform**: Real-time community interaction
- **Mailing List**: Announcements and summaries
- **Regular Calls**: Community and working group meetings

### Community Events

- Monthly community calls
- Quarterly roadmap planning sessions
- Annual GUARDRAIL Summit
- Regional meetups and workshops

### Code of Conduct

All participants in the GUARDRAIL community are expected to adhere to the Code of Conduct, which promotes respectful and inclusive participation. The Code of Conduct is enforced by a Community Committee with a defined reporting and resolution process.

## Project Resources

### Official Resources

- **GitHub Organization**: Primary home for all project repositories
- **Documentation Site**: Comprehensive guides and references
- **Website**: Project information and resources
- **CI/CD Infrastructure**: Testing and deployment pipelines

### Resource Management

- **Budget Decisions**: Managed by the TSC with transparency
- **Infrastructure Access**: Based on role and need
- **Official Communications**: Managed by Community Working Group

## Intellectual Property

### Licensing

- **Code**: MIT License
- **Documentation**: Creative Commons Attribution 4.0 International (CC BY 4.0)
- **Specification**: Open specification without restrictions

### Contributor Agreements

- **Developer Certificate of Origin (DCO)**: Sign-off on all contributions
- **No CLA**: To reduce contribution barriers

### Trademark

- GUARDRAIL name and logo are project trademarks
- Usage guidelines published for community use
- TSC manages trademark enforcement

## Evolution of Governance

This governance model is designed to evolve as the project grows:

### Amendment Process

1. Propose governance changes as formal GRIPs
2. Community discussion period (minimum 30 days)
3. TSC vote on governance changes
4. Regular governance review (annual)

### Dispute Resolution

1. Working group level resolution attempt
2. Escalation to TSC if unresolved
3. Community-wide discussion for significant disputes
4. Final decision by TSC vote

## Initial Implementation

### Bootstrap Phase (First 6 Months)

1. **Founding TSC**: Initial committee of 5 members from founding organizations
2. **Core Maintainers**: Initial set of component maintainers
3. **Governance Iteration**: First governance review after 6 months
4. **Community Building**: Active outreach and contributor recruitment

### Year 1 Milestones

- Establish all working groups
- Hold first TSC elections
- Publish formal specification
- Build active contributor community
- Complete initial governance review

## Appendices

### A. Technical Steering Committee Charter

Detailed responsibilities, authorities, meeting procedures, and election process for the TSC.

### B. Working Group Framework

Working group formation, roles, responsibilities, and operating procedures.

### C. GRIP Process and Template

Complete process description and template for GUARDRAIL Improvement Proposals.

### D. Code of Conduct

Behavioral expectations, reporting process, and enforcement mechanisms.

### E. Maintainer Guidelines

Detailed responsibilities, review standards, and maintainer selection criteria.