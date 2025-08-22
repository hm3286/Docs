# Salesforce Architecture Diagram (Mermaid)

```mermaid
flowchart TB
    %% USER LAYER
    subgraph Users[End Users & Clients]
        BROWSER[Browser]
        MOBILE[Mobile App]
        API[External API Client]
    end

    %% APP SERVER LAYER
    subgraph AppLayer[App Server Layer]
        APP1["App Server Cluster<br/>(Apex Runtime, Lightning UI, APIs)"]
        FALCON["Falcon Runtime<br/>(Next-gen optimized engine)"]
    end

    %% CORE PLATFORM
    subgraph Core[Core Platform Services]
        META["Metadata Engine<br/>(Configuration-driven runtime)"]
        SEC["Security & Sharing Rules"]
        WF["Workflow & Process Automation"]
        API_GW["API Gateway<br/>(SOAP/REST/Bulk/GraphQL)"]
    end

    %% NEAR CORE
    subgraph NearCore[Near-Core Services]
        EINSTEIN["Einstein AI / Analytics"]
        MC["Marketing Cloud"]
        MULE["MuleSoft Integrations"]
        IND["Industry Clouds"]
    end

    %% POD + DB
    subgraph POD["POD - Point of Delivery"]
        LB["Load Balancer"]
        subgraph AppCluster["App Servers"]
            AS1["App Server Node 1"]
            AS2["App Server Node 2"]
            AS3["App Server Node N"]
        end
        subgraph DBCluster["Multi-tenant Database Cluster"]
            DB_A["DB Shard A<br/>(OrgID Partition)"]
            DB_B["DB Shard B<br/>(OrgID Partition)"]
            DB_C["DB Shard C<br/>(OrgID Partition)"]
        end
        SEARCH["Search Servers<br/>(Elasticsearch)"]
        CACHE["Cache Layer<br/>(Redis-like)"]
    end

    %% INFRA
    subgraph Infra["Infrastructure Layer"]
        SFDC_DC["Salesforce Data Center - Classic"]
        HYPERFORCE["Hyperforce (AWS/Azure/GCP)<br/>Cloud-native, Postgres, K8s"]
    end

    %% CONNECTIONS
    BROWSER --> APP1
    MOBILE --> APP1
    API --> APP1

    APP1 --> FALCON
    APP1 --> META
    APP1 --> SEC
    APP1 --> WF
    APP1 --> API_GW

    Core --> NearCore

    APP1 --> LB
    LB --> AS1
    LB --> AS2
    LB --> AS3

    AS1 --> DB_A
    AS2 --> DB_B
    AS3 --> DB_C

    DB_A --- SEARCH
    DB_B --- SEARCH
    DB_C --- SEARCH
    DB_A --- CACHE
    DB_B --- CACHE
    DB_C --- CACHE

    POD --> Infra
    Infra --> SFDC_DC
    Infra --> HYPERFORCE
