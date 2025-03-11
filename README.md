# GUARDRAIL

GUARDRAIL (Gateway for Unified Access, Resource Delegation, and Risk-Attenuating Information Limits) is a comprehensive security framework for Large Language Model (LLM) application ecosystems using the Model Context Protocol (MCP).

## Overview

GUARDRAIL implements a multi-layered security architecture to:
- Control information flows between boundaries
- Verify execution contexts
- Manage resource access
- Contain execution environments
- Provide comprehensive auditing and monitoring

The framework can be deployed in multiple configurations:
- **Embedded Model**: Components integrated directly into the host process
- **Gateway Model**: Standalone security gateway mediating all MCP traffic
- **Service Mesh Model**: Implemented as sidecars in Kubernetes pods with a central control plane

## Core Principles

1. **Information Flow Control**
   - Strict policies governing information movement between boundaries
   - Resource classification and labeling
   - Fine-grained control over data access and propagation

2. **Contextual Security**
   - Security decisions based on execution context
   - Dynamic trust assessment
   - Environmental awareness

3. **Transport-Agnostic Protection**
   - Security guarantees regardless of transport mechanism
   - Protocol-level safeguards
   - Resilience against transport compromise

4. **Least-Privilege Execution**
   - Minimal permissions for operation
   - Just-in-time privilege granting
   - Automatic privilege revocation

## Architecture

GUARDRAIL is structured into five hierarchical security layers:

### 1. Information Gateway Layer
- Flow Policy Engine: Defines and enforces rules for information movement
- Content Classification: Labels information based on sensitivity
- Transport Encapsulation: Secures data regardless of underlying transport

### 2. Context Verification Layer
- Runtime Attestation: Verifies the integrity of the execution environment
- Client/Server Verification: Establishes identity and authenticity of endpoints
- Policy Discovery: Determines applicable security policies for the context

### 3. Request Control Layer
- Request Filter: Validates incoming requests against security policies
- Resource Guard: Controls access to sensitive resources
- Action Limiter: Restricts operations based on context and permissions

### 4. Execution Containment Layer
- Memory Isolation: Prevents unauthorized memory access
- Resource Quotas: Limits resource consumption
- Call Chain Tracking: Monitors execution paths for policy violations

### 5. Audit and Monitoring Layer
- Flow Logging: Records all information transfers
- Anomaly Detection: Identifies suspicious patterns
- Tamper-Evident Records: Creates immutable audit trails

## Visualizations and Diagrams

The following diagrams provide visual representations of GUARDRAIL's architecture, deployment models, and internal components. These visualizations aid in understanding the system's structure and operation.

- **Embedded Deployment Model:**
  ![Embedded Deployment Model](svgImages/6.png)

- **Gateway Deployment Model:**
  ![Gateway Deployment Model](svgImages/8.png)

- **Gateway Internal Architecture:**
    ![Gateway Internal Architecture](svgImages/9.png)

- **Service Mesh Deployment Model (First Instance):**
   ![Service Mesh Deployment Model](svgImages/3.png)
   
- **Service Mesh Deployment Model (Duplicate Instance):**
   ![Service Mesh Deployment Model](svgImages/7.png)
- **Gateway Deployment Model (Duplicate Instance):**
   ![Gateway Deployment Model](svgImages/5.png)
- **Gateway Internal Architecture (Duplicate instance):**
   ![Gateway Internal Architecture](svgImages/4.png)

- **Gateway Data Flow Architecture:**
  ![Gateway Data Flow Architecture](svgImages/10.png)

- **Gateway 19" Rack Appliance Interface:**
  ![Gateway 19 Rack Appliance Interface](svgImages/11.png)

- **Sidecar Internal Architecture:**
  ![Sidecar Internal Architecture](svgImages/12.png)

- **Service Mesh Containerized Architecture:**
  ![Service Mesh Containerized Architecture](svgImages/13.png)

- **Service Mesh Control Plane Architecture:**
  ![Service Mesh Control Plane Architecture](svgImages/14.png)



## Integration with MCP

GUARDRAIL integrates with the Model Context Protocol (MCP) to provide security for LLM applications:

```typescript
// Client Integration Example
import { Client } from "@modelcontextprotocol/sdk/client";
import { GuardrailClient } from "@guardrail/sdk/client";

// Initialize standard MCP client
const mcpClient = new Client({
  name: "example-client",
  version: "1.0.0"
});

// Wrap with Guardrail protection
const client = new GuardrailClient(mcpClient, {
  security: {
    classification_level: "INTERNAL",
    flow_policies: "standard",
    attestation: true
  }
});

// Normal MCP operations now protected by Guardrail
await client.connect(transport);
```

## Key Benefits

1. **Complete Information Flow Control**
   - Prevents unauthorized data exfiltration
   - Controls information infiltration
   - Enforces classification-based rules

2. **Contextual Security Model**
   - Security adapts to execution environment
   - Dynamic trust assessment
   - Environment-aware policies

3. **Preservation of MCP Functionality**
   - Compatible with existing MCP implementations
   - Minimal performance overhead
   - Transparent to well-behaved applications

4. **Defense Against Common Attack Vectors**
   - Protection against prompt injection
   - Prevention of resource abuse
   - Mitigation of side-channel attacks

5. **Comprehensive Audit Trail**
   - Complete visibility into information flows
   - Evidence for security investigations
   - Compliance with data protection requirements

## Documentation

For more detailed information, please refer to:
- [Technical Specification](./2-technical-spec.md)
- [Architecture Diagrams](./6-diags-mermaid.md)
- [Gateway Architecture Diagrams](./4-diag-gateway.svg.txt)
- [Service Mesh Diagrams](./5-diag-service-mesh.svg.txt)

## Emergency Response Framework

GUARDRAIL incorporates an Emergency Response Framework providing comprehensive procedures to detect, respond to, and recover from security incidents. For more information, see [Emergency Response Framework](./SHIELD-7-emergency-response-framework.md).

## Project Status

GUARDRAIL is currently in active development. This repository contains the architectural design, technical specifications, and implementation documentation. Production-ready code components will be released incrementally.

## License

This project is licensed under the [MIT License](./LICENSE).

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.