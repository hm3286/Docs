```mermaid
graph TD
  subgraph Producers
    PRD1["Producer 1"]
    PRD2["Producer 2"]
  end

  subgraph Brokers
    B1["Broker 1"]
    B2["Broker 2"]
    B3["Broker 3"]
  end

  subgraph Topic["Topic: AIRPORT_ARRIVALS"]
    P1["Partition 1\n(Leader: Broker 1)"]
    P2["Partition 2\n(Leader: Broker 2)"]
    P3["Partition 3\n(Leader: Broker 3)"]
  end

  subgraph Replication
    P1R2["Partition 1 Replica\n(Follower: Broker 2)"]
    P1R3["Partition 1 Replica\n(Follower: Broker 3)"]
    P2R1["Partition 2 Replica\n(Follower: Broker 1)"]
    P2R3["Partition 2 Replica\n(Follower: Broker 3)"]
    P3R1["Partition 3 Replica\n(Follower: Broker 1)"]
    P3R2["Partition 3 Replica\n(Follower: Broker 2)"]
  end

  subgraph Consumers["Consumer Group"]
    C1["Consumer 1"]
    C2["Consumer 2"]
    C3["Consumer 3"]
  end

  subgraph ZK["ZooKeeper"]
    ZK1["ZooKeeper"]
  end

  PRD1-->|Publishes|Topic
  PRD2-->|Publishes|Topic

  Topic-->|Partitioning|P1
  Topic-->|Partitioning|P2
  Topic-->|Partitioning|P3

  P1-->|Stored On|B1
  P2-->|Stored On|B2
  P3-->|Stored On|B3

  P1-->|Replica|P1R2
  P1-->|Replica|P1R3
  P2-->|Replica|P2R1
  P2-->|Replica|P2R3
  P3-->|Replica|P3R1
  P3-->|Replica|P3R2

  P1R2-->|Stored On|B2
  P1R3-->|Stored On|B3
  P2R1-->|Stored On|B1
  P2R3-->|Stored On|B3
  P3R1-->|Stored On|B1
  P3R2-->|Stored On|B2

  C1-->|Reads|P1
  C2-->|Reads|P2
  C3-->|Reads|P3

  ZK1-->|Coordinates|B1
  ZK1-->|Coordinates|B2
  ZK1-->|Coordinates|B3