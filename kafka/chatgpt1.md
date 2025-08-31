```mermaid
flowchart TB

    %% Producer
    subgraph Producers
        P1[Producer]
    end

    %% Kafka Cluster with Brokers and Partitions (Replication)
    subgraph KafkaCluster["Kafka Cluster (3 Brokers)"]

        subgraph Broker1["Broker 1"]
            P0L[Partition 0 (Leader)]
            P1F[Partition 1 (Follower)]
            P2F[Partition 2 (Follower)]
        end

        subgraph Broker2["Broker 2"]
            P0F[Partition 0 (Follower)]
            P1L[Partition 1 (Leader)]
            P2F2[Partition 2 (Follower)]
        end

        subgraph Broker3["Broker 3"]
            P0F2[Partition 0 (Follower)]
            P1F2[Partition 1 (Follower)]
            P2L[Partition 2 (Leader)]
        end
    end

    %% Consumer Group 1 (3 Consumers, 1-to-1 mapping)
    subgraph ConsumerGroup1["Consumer Group 1"]
        C1A[Consumer 1A]
        C1B[Consumer 1B]
        C1C[Consumer 1C]
    end

    %% Consumer Group 2 (2 Consumers, fewer than partitions)
    subgraph ConsumerGroup2["Consumer Group 2"]
        C2A[Consumer 2A]
        C2B[Consumer 2B]
    end

    %% Producer writes only to Leaders
    P1 -->|orderId=101 → Partition 0| P0L
    P1 -->|orderId=202 → Partition 1| P1L
    P1 -->|orderId=303 → Partition 2| P2L

    %% Followers replicate from leaders
    P0L -.replicates.-> P0F
    P0L -.replicates.-> P0F2
    P1L -.replicates.-> P1F
    P1L -.replicates.-> P1F2
    P2L -.replicates.-> P2F
    P2L -.replicates.-> P2F2

    %% Consumer Group 1 assignments
    P0L -->|"order101 FIFO"| C1A
    P1L -->|"order202 FIFO"| C1B
    P2L -->|"order303 FIFO"| C1C

    %% Consumer Group 2 assignments (less consumers than partitions)
    P0L -->|"order101 FIFO"| C2A
    P1L -->|"order202 FIFO"| C2B
    P2L -->|"order303 FIFO"| C2B
